# Fan Token Dashboard: Real Data Implementation Guide

## Overview
Transform the mock data dashboard into a production-ready system that fetches real-time data from the Chiliz blockchain.

## Architecture Options

### Option 1: Direct Blockchain Connection (Client-Side)
**Best for**: Decentralized, trustless access
**Complexity**: Medium
**Cost**: Free (using public RPCs)

### Option 2: Backend API + Database
**Best for**: Performance, historical data, complex queries
**Complexity**: High
**Cost**: Server + Database costs

### Option 3: Hybrid (Indexed Data + Live Updates)
**Best for**: Best of both worlds
**Complexity**: Medium-High
**Cost**: Moderate

---

## Option 1: Direct Blockchain Implementation

### Step 1: Create Blockchain Service Module

```javascript
// blockchain-service.js
import { ethers } from 'ethers';

class ChilizBlockchainService {
    constructor() {
        // Multiple RPC endpoints for redundancy
        this.providers = [
            'https://rpc.ankr.com/chiliz',
            'https://chiliz.publicnode.com',
            'https://chiliz-mainnet.gateway.tatum.io'
        ];
        
        this.provider = new ethers.JsonRpcProvider(this.providers[0]);
        this.contracts = {};
        this.initializeContracts();
    }
    
    initializeContracts() {
        const ERC20_ABI = [
            'function balanceOf(address owner) view returns (uint256)',
            'function totalSupply() view returns (uint256)',
            'function decimals() view returns (uint8)',
            'event Transfer(address indexed from, address indexed to, uint256 value)'
        ];
        
        // Initialize all Brazilian token contracts
        const tokens = {
            MENGO: '0xd1723eb9e7c6ee7c7e2d421b2758dc0f2166eddc',
            SCCP: '0x20BFeab58f8bE903753d037Ba7e307fc77c97388',
            VERDAO: '0x971364Ec452958d4D65Ba8D508FAa226d7117279',
            FLU: '0x86930777d43605C40bA786F7802778ff5413eFaB',
            BAHIA: '0xE92e152fC0ff1368739670a5175175154Ceeef42',
            MIBR: '0xa8206Af1e6a0289156d45B9d60e5bbD5d1fCf68d',
            VASCO: '0x689dee82d191dac845599f46393c2efe4b5896a2'
        };
        
        Object.entries(tokens).forEach(([symbol, address]) => {
            this.contracts[symbol] = new ethers.Contract(
                address,
                ERC20_ABI,
                this.provider
            );
        });
    }
    
    async getTokenTransfers(tokenSymbol, fromBlock, toBlock) {
        const contract = this.contracts[tokenSymbol];
        const filter = contract.filters.Transfer();
        
        const logs = await contract.queryFilter(filter, fromBlock, toBlock);
        
        return logs.map(log => ({
            from: log.args.from,
            to: log.args.to,
            value: ethers.formatUnits(log.args.value, 0), // 0 decimals for fan tokens
            blockNumber: log.blockNumber,
            transactionHash: log.transactionHash,
            timestamp: null // Will fetch separately
        }));
    }
    
    async getBlockTimestamps(blockNumbers) {
        const timestamps = {};
        const uniqueBlocks = [...new Set(blockNumbers)];
        
        for (const blockNum of uniqueBlocks) {
            const block = await this.provider.getBlock(blockNum);
            timestamps[blockNum] = block.timestamp * 1000; // Convert to milliseconds
        }
        
        return timestamps;
    }
    
    async getDailyVolume(tokenSymbol, date) {
        // Calculate block range for the date
        const startTime = new Date(date).getTime() / 1000;
        const endTime = startTime + 86400; // 24 hours
        
        // Get approximate blocks (Chiliz has ~2 second blocks)
        const currentBlock = await this.provider.getBlockNumber();
        const blocksPerDay = 43200; // Approximate
        
        const transfers = await this.getTokenTransfers(
            tokenSymbol,
            currentBlock - blocksPerDay,
            currentBlock
        );
        
        // Calculate volume
        const volume = transfers.reduce((sum, transfer) => {
            return sum + BigInt(transfer.value);
        }, 0n);
        
        return {
            token: tokenSymbol,
            date: date,
            volume: volume.toString(),
            transactionCount: transfers.length,
            uniqueAddresses: new Set([
                ...transfers.map(t => t.from),
                ...transfers.map(t => t.to)
            ]).size
        };
    }
    
    async getCurrentPrice(tokenSymbol) {
        // Note: Would need DEX integration or price oracle
        // For now, we can track CHZ pair prices on Chiliz DEX
        // This is a simplified example
        
        const CHILIZ_DEX_ROUTER = '0x...'; // Would need actual address
        // Implementation would query DEX for current price
        
        return {
            token: tokenSymbol,
            priceInCHZ: 0,
            priceInUSD: 0,
            timestamp: Date.now()
        };
    }
}
```

### Step 2: Create Data Fetching Layer

```javascript
// data-fetcher.js
class FanTokenDataFetcher {
    constructor() {
        this.blockchain = new ChilizBlockchainService();
        this.cache = new Map();
        this.cacheTimeout = 60000; // 1 minute cache
    }
    
    async fetchRealTimeData(tokens, dateRange) {
        const results = [];
        
        for (const token of tokens) {
            const cacheKey = `${token}-${dateRange.start}-${dateRange.end}`;
            
            // Check cache first
            if (this.cache.has(cacheKey)) {
                const cached = this.cache.get(cacheKey);
                if (Date.now() - cached.timestamp < this.cacheTimeout) {
                    results.push(cached.data);
                    continue;
                }
            }
            
            // Fetch from blockchain
            const data = await this.fetchTokenData(token, dateRange);
            
            // Update cache
            this.cache.set(cacheKey, {
                data: data,
                timestamp: Date.now()
            });
            
            results.push(data);
        }
        
        return results;
    }
    
    async fetchTokenData(token, dateRange) {
        const days = this.getDaysInRange(dateRange.start, dateRange.end);
        const dailyData = [];
        
        for (const day of days) {
            const dayData = await this.blockchain.getDailyVolume(token, day);
            dailyData.push(dayData);
        }
        
        return {
            token: token,
            data: dailyData,
            summary: this.calculateSummary(dailyData)
        };
    }
    
    getDaysInRange(start, end) {
        const days = [];
        const current = new Date(start);
        const endDate = new Date(end);
        
        while (current <= endDate) {
            days.push(current.toISOString().split('T')[0]);
            current.setDate(current.getDate() + 1);
        }
        
        return days;
    }
    
    calculateSummary(dailyData) {
        return {
            totalVolume: dailyData.reduce((sum, d) => sum + BigInt(d.volume), 0n).toString(),
            totalTransactions: dailyData.reduce((sum, d) => sum + d.transactionCount, 0),
            avgDailyVolume: (BigInt(dailyData.reduce((sum, d) => sum + BigInt(d.volume), 0n)) / BigInt(dailyData.length)).toString(),
            peakDay: dailyData.reduce((max, d) => BigInt(d.volume) > BigInt(max.volume) ? d : max, dailyData[0])
        };
    }
}
```

### Step 3: Integrate with Dashboard

```javascript
// dashboard-integration.js
class RealDataDashboard {
    constructor() {
        this.fetcher = new FanTokenDataFetcher();
        this.updateInterval = null;
        this.isLive = false;
    }
    
    async initialize() {
        // Show loading state
        this.showLoading(true);
        
        try {
            // Fetch initial data for all tokens
            const tokens = ['MENGO', 'SCCP', 'VERDAO', 'FLU', 'BAHIA', 'MIBR', 'VASCO'];
            const dateRange = {
                start: new Date(Date.now() - 30 * 24 * 60 * 60 * 1000).toISOString().split('T')[0],
                end: new Date().toISOString().split('T')[0]
            };
            
            const data = await this.fetcher.fetchRealTimeData(tokens, dateRange);
            this.updateCharts(data);
            
        } catch (error) {
            console.error('Failed to fetch blockchain data:', error);
            this.showError('Failed to connect to Chiliz blockchain');
        } finally {
            this.showLoading(false);
        }
    }
    
    enableLiveMode() {
        this.isLive = true;
        
        // Update every 30 seconds
        this.updateInterval = setInterval(async () => {
            await this.fetchLatestData();
        }, 30000);
        
        // Also set up WebSocket for real-time events
        this.setupWebSocket();
    }
    
    setupWebSocket() {
        const ws = new WebSocket('wss://chiliz.publicnode.com');
        
        // Subscribe to Transfer events for all tokens
        const tokens = Object.values(this.contracts);
        
        tokens.forEach(contract => {
            contract.on('Transfer', (from, to, value) => {
                this.handleTransferEvent({
                    token: contract.address,
                    from: from,
                    to: to,
                    value: value.toString(),
                    timestamp: Date.now()
                });
            });
        });
    }
    
    handleTransferEvent(event) {
        // Update live statistics
        this.updateLiveStats(event);
        
        // Add to recent transactions feed
        this.addToTransactionFeed(event);
        
        // Update volume charts if needed
        if (this.shouldUpdateChart(event)) {
            this.updateVolumeChart(event);
        }
    }
}
```

---

## Option 2: Backend API Implementation

### Backend Service (Node.js + Express)

```javascript
// server.js
import express from 'express';
import { ethers } from 'ethers';
import postgres from 'pg';
import Redis from 'redis';
import cron from 'node-cron';

const app = express();
const { Pool } = postgres;

// Database setup
const db = new Pool({
    connectionString: process.env.DATABASE_URL
});

// Redis cache
const redis = Redis.createClient({
    url: process.env.REDIS_URL
});

// Blockchain indexer
class BlockchainIndexer {
    constructor() {
        this.provider = new ethers.JsonRpcProvider('https://rpc.ankr.com/chiliz');
        this.lastIndexedBlock = 0;
    }
    
    async startIndexing() {
        // Get last indexed block from database
        const result = await db.query('SELECT MAX(block_number) as last_block FROM transfers');
        this.lastIndexedBlock = result.rows[0].last_block || 0;
        
        // Start indexing from last block
        this.provider.on('block', async (blockNumber) => {
            await this.indexBlock(blockNumber);
        });
        
        // Also run historical indexing in background
        this.indexHistoricalData();
    }
    
    async indexBlock(blockNumber) {
        const block = await this.provider.getBlock(blockNumber, true);
        
        for (const tx of block.transactions) {
            if (this.isTokenTransfer(tx)) {
                await this.saveTransfer(tx, block.timestamp);
            }
        }
        
        // Update last indexed block
        await db.query('UPDATE indexer_state SET last_block = $1', [blockNumber]);
    }
    
    async saveTransfer(tx, timestamp) {
        const decoded = this.decodeTransfer(tx);
        
        await db.query(`
            INSERT INTO transfers (
                token_address, from_address, to_address, 
                value, block_number, tx_hash, timestamp
            ) VALUES ($1, $2, $3, $4, $5, $6, $7)
        `, [
            decoded.token,
            decoded.from,
            decoded.to,
            decoded.value,
            tx.blockNumber,
            tx.hash,
            new Date(timestamp * 1000)
        ]);
    }
}

// API Endpoints
app.get('/api/tokens/:symbol/volume', async (req, res) => {
    const { symbol } = req.params;
    const { start, end } = req.query;
    
    // Check Redis cache first
    const cacheKey = `volume:${symbol}:${start}:${end}`;
    const cached = await redis.get(cacheKey);
    
    if (cached) {
        return res.json(JSON.parse(cached));
    }
    
    // Query database
    const result = await db.query(`
        SELECT 
            DATE(timestamp) as date,
            SUM(value) as volume,
            COUNT(*) as transaction_count,
            COUNT(DISTINCT from_address) + COUNT(DISTINCT to_address) as unique_users
        FROM transfers
        WHERE token_address = $1
            AND timestamp >= $2
            AND timestamp <= $3
        GROUP BY DATE(timestamp)
        ORDER BY date
    `, [TOKEN_ADDRESSES[symbol], start, end]);
    
    // Cache for 5 minutes
    await redis.setex(cacheKey, 300, JSON.stringify(result.rows));
    
    res.json(result.rows);
});

app.get('/api/tokens/:symbol/holders', async (req, res) => {
    const { symbol } = req.params;
    
    // This would require maintaining a balance table
    const result = await db.query(`
        SELECT COUNT(DISTINCT address) as holder_count
        FROM token_balances
        WHERE token_address = $1 AND balance > 0
    `, [TOKEN_ADDRESSES[symbol]]);
    
    res.json(result.rows[0]);
});

app.get('/api/stats/summary', async (req, res) => {
    const stats = await db.query(`
        SELECT 
            t.symbol,
            SUM(tr.value) as total_volume,
            COUNT(*) as total_transactions,
            COUNT(DISTINCT tr.from_address) + COUNT(DISTINCT tr.to_address) as total_users
        FROM transfers tr
        JOIN tokens t ON tr.token_address = t.address
        WHERE tr.timestamp >= NOW() - INTERVAL '24 hours'
        GROUP BY t.symbol
    `);
    
    res.json(stats.rows);
});

// WebSocket for real-time updates
import { WebSocketServer } from 'ws';

const wss = new WebSocketServer({ port: 8080 });

wss.on('connection', (ws) => {
    // Send initial data
    ws.send(JSON.stringify({ type: 'connected' }));
    
    // Subscribe to new transfers
    subscribeToTransfers(ws);
});

function subscribeToTransfers(ws) {
    // Listen for new transfers and broadcast
    provider.on('Transfer', (event) => {
        ws.send(JSON.stringify({
            type: 'transfer',
            data: event
        }));
    });
}

// Start server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`API running on port ${PORT}`);
    
    // Start blockchain indexer
    const indexer = new BlockchainIndexer();
    indexer.startIndexing();
    
    // Run aggregation jobs
    cron.schedule('*/5 * * * *', updateAggregatedStats);
});
```

### Database Schema

```sql
-- PostgreSQL schema
CREATE TABLE tokens (
    address VARCHAR(42) PRIMARY KEY,
    symbol VARCHAR(10) NOT NULL,
    name VARCHAR(100) NOT NULL,
    total_supply NUMERIC(78, 0),
    decimals INTEGER DEFAULT 0
);

CREATE TABLE transfers (
    id SERIAL PRIMARY KEY,
    token_address VARCHAR(42) NOT NULL,
    from_address VARCHAR(42) NOT NULL,
    to_address VARCHAR(42) NOT NULL,
    value NUMERIC(78, 0) NOT NULL,
    block_number INTEGER NOT NULL,
    tx_hash VARCHAR(66) NOT NULL,
    timestamp TIMESTAMP NOT NULL,
    FOREIGN KEY (token_address) REFERENCES tokens(address)
);

CREATE TABLE token_balances (
    address VARCHAR(42) NOT NULL,
    token_address VARCHAR(42) NOT NULL,
    balance NUMERIC(78, 0) NOT NULL,
    last_updated TIMESTAMP NOT NULL,
    PRIMARY KEY (address, token_address)
);

CREATE TABLE daily_stats (
    date DATE NOT NULL,
    token_address VARCHAR(42) NOT NULL,
    volume NUMERIC(78, 0) NOT NULL,
    transaction_count INTEGER NOT NULL,
    unique_users INTEGER NOT NULL,
    avg_transaction_size NUMERIC(78, 0),
    PRIMARY KEY (date, token_address)
);

-- Indexes for performance
CREATE INDEX idx_transfers_token_timestamp ON transfers(token_address, timestamp);
CREATE INDEX idx_transfers_block ON transfers(block_number);
CREATE INDEX idx_balances_token ON token_balances(token_address);
```

---

## Option 3: Using Third-Party Services

### Chiliscan API Integration

```javascript
// chiliscan-api.js
class ChiliscanAPI {
    constructor(apiKey = null) {
        this.baseURL = 'https://chiliscan.com/api';
        this.apiKey = apiKey; // Optional, higher rate limits with key
    }
    
    async getTokenTransfers(tokenAddress, page = 1) {
        const url = `${this.baseURL}/token/transfers`;
        const params = {
            contractaddress: tokenAddress,
            page: page,
            offset: 100,
            sort: 'desc'
        };
        
        if (this.apiKey) {
            params.apikey = this.apiKey;
        }
        
        const response = await fetch(`${url}?${new URLSearchParams(params)}`);
        return response.json();
    }
    
    async getTokenHolders(tokenAddress) {
        const url = `${this.baseURL}/token/holders`;
        const params = {
            contractaddress: tokenAddress
        };
        
        if (this.apiKey) {
            params.apikey = this.apiKey;
        }
        
        const response = await fetch(`${url}?${new URLSearchParams(params)}`);
        return response.json();
    }
    
    async getTokenInfo(tokenAddress) {
        const url = `${this.baseURL}/token/info`;
        const params = {
            contractaddress: tokenAddress
        };
        
        const response = await fetch(`${url}?${new URLSearchParams(params)}`);
        return response.json();
    }
}

// Usage in dashboard
async function fetchDataFromChiliscan() {
    const api = new ChiliscanAPI();
    const tokens = {
        MENGO: '0xd1723eb9e7c6ee7c7e2d421b2758dc0f2166eddc',
        // ... other tokens
    };
    
    const data = {};
    
    for (const [symbol, address] of Object.entries(tokens)) {
        // Get transfers
        const transfers = await api.getTokenTransfers(address);
        
        // Get holder count
        const holders = await api.getTokenHolders(address);
        
        // Get token info
        const info = await api.getTokenInfo(address);
        
        data[symbol] = {
            transfers: transfers.result,
            holderCount: holders.result.length,
            info: info.result
        };
    }
    
    return data;
}
```

---

## Frontend Integration

### Update Dashboard HTML

```html
<!-- Add to fan-token-dashboard.html -->
<script src="https://cdn.ethers.io/lib/ethers-5.0.umd.min.js"></script>

<script>
// Real data mode toggle
let dataMode = 'mock'; // 'mock' or 'real'
let dataService = null;

async function initializeRealDataMode() {
    // Check if MetaMask or other wallet is available
    if (typeof window.ethereum !== 'undefined') {
        try {
            // Request account access
            await window.ethereum.request({ method: 'eth_requestAccounts' });
            
            // Create ethers provider
            const provider = new ethers.providers.Web3Provider(window.ethereum);
            
            // Switch to Chiliz network if needed
            await switchToChilizNetwork();
            
            // Initialize data service
            dataService = new ChilizDataService(provider);
            
            // Update UI
            document.getElementById('connectionStatus').textContent = 'Connected to Chiliz';
            dataMode = 'real';
            
            // Fetch real data
            await fetchRealData();
            
        } catch (error) {
            console.error('Failed to connect:', error);
            alert('Failed to connect to Chiliz network');
        }
    } else {
        alert('Please install MetaMask to use real data mode');
    }
}

async function switchToChilizNetwork() {
    try {
        await window.ethereum.request({
            method: 'wallet_switchEthereumChain',
            params: [{ chainId: '0x15B38' }], // 88888 in hex
        });
    } catch (error) {
        if (error.code === 4902) {
            // Chain not added, add it
            await window.ethereum.request({
                method: 'wallet_addEthereumChain',
                params: [{
                    chainId: '0x15B38',
                    chainName: 'Chiliz Chain',
                    nativeCurrency: {
                        name: 'CHZ',
                        symbol: 'CHZ',
                        decimals: 18
                    },
                    rpcUrls: ['https://rpc.ankr.com/chiliz'],
                    blockExplorerUrls: ['https://chiliscan.com']
                }]
            });
        }
    }
}

class ChilizDataService {
    constructor(provider) {
        this.provider = provider;
        this.contracts = {};
        this.initContracts();
    }
    
    initContracts() {
        const ERC20_ABI = [
            'function totalSupply() view returns (uint256)',
            'function balanceOf(address) view returns (uint256)',
            'event Transfer(address indexed from, address indexed to, uint256 value)'
        ];
        
        const tokens = {
            MENGO: '0xd1723eb9e7c6ee7c7e2d421b2758dc0f2166eddc',
            SCCP: '0x20BFeab58f8bE903753d037Ba7e307fc77c97388',
            // ... other tokens
        };
        
        Object.entries(tokens).forEach(([symbol, address]) => {
            this.contracts[symbol] = new ethers.Contract(
                address,
                ERC20_ABI,
                this.provider
            );
        });
    }
    
    async getRecentTransfers(symbol, blocks = 1000) {
        const contract = this.contracts[symbol];
        const currentBlock = await this.provider.getBlockNumber();
        
        const filter = contract.filters.Transfer();
        const events = await contract.queryFilter(
            filter,
            currentBlock - blocks,
            currentBlock
        );
        
        return events.map(event => ({
            from: event.args.from,
            to: event.args.to,
            value: ethers.utils.formatUnits(event.args.value, 0),
            blockNumber: event.blockNumber,
            transactionHash: event.transactionHash
        }));
    }
}

async function fetchRealData() {
    if (!dataService) return;
    
    showLoading(true);
    
    try {
        const data = [];
        
        for (const symbol of Object.keys(BRAZILIAN_TOKENS)) {
            const transfers = await dataService.getRecentTransfers(symbol);
            
            // Process transfers into daily volumes
            const dailyVolumes = processToDailyVolumes(transfers);
            
            data.push({
                token: symbol,
                data: dailyVolumes
            });
        }
        
        // Update charts with real data
        updateChartsWithRealData(data);
        
    } catch (error) {
        console.error('Error fetching real data:', error);
    } finally {
        showLoading(false);
    }
}

function processToDailyVolumes(transfers) {
    const volumes = {};
    
    transfers.forEach(transfer => {
        const date = new Date(transfer.blockNumber * 2000).toISOString().split('T')[0]; // Approximate
        
        if (!volumes[date]) {
            volumes[date] = {
                volume: 0,
                transactions: 0
            };
        }
        
        volumes[date].volume += parseFloat(transfer.value);
        volumes[date].transactions += 1;
    });
    
    return volumes;
}

// Add button to switch modes
function addDataModeToggle() {
    const button = document.createElement('button');
    button.className = 'btn';
    button.textContent = '🔄 Switch to Real Data';
    button.onclick = async () => {
        if (dataMode === 'mock') {
            await initializeRealDataMode();
        } else {
            dataMode = 'mock';
            document.getElementById('connectionStatus').textContent = 'Mock Data Mode';
            refreshData();
        }
    };
    
    document.querySelector('.data-section').appendChild(button);
}

// Initialize on load
document.addEventListener('DOMContentLoaded', () => {
    addDataModeToggle();
});
</script>
```

---

## Deployment Options

### 1. Static Hosting (Client-Side Only)
- **Vercel/Netlify**: Deploy the HTML file with client-side blockchain access
- **IPFS**: Decentralized hosting
- **GitHub Pages**: Free static hosting

### 2. Full Stack Deployment
- **Backend**: Heroku, Railway, or AWS EC2
- **Database**: PostgreSQL on Supabase or AWS RDS
- **Cache**: Redis Cloud
- **Frontend**: Vercel or Netlify

### 3. Serverless Architecture
- **Functions**: Vercel Functions or AWS Lambda
- **Database**: PlanetScale or FaunaDB
- **Cache**: Upstash Redis

---

## Cost Estimates

### Minimal (Client-Side Only)
- **Hosting**: Free (Vercel/Netlify)
- **RPC Access**: Free (public endpoints)
- **Total**: $0/month

### Professional (With Backend)
- **Backend Server**: $20/month (Railway)
- **Database**: $25/month (Supabase)
- **Redis Cache**: $10/month (Upstash)
- **Domain**: $12/year
- **Total**: ~$56/month

### Enterprise (High Performance)
- **Load Balanced Servers**: $200/month
- **Managed Database**: $100/month
- **CDN**: $50/month
- **Monitoring**: $50/month
- **Total**: ~$400/month

---

## Next Steps

1. **Choose Architecture**: Based on requirements and budget
2. **Set Up Infrastructure**: Deploy backend if needed
3. **Implement Data Service**: Connect to Chiliz blockchain
4. **Add Caching Layer**: Optimize performance
5. **Implement Error Handling**: Ensure reliability
6. **Add Monitoring**: Track performance and errors
7. **Deploy to Production**: Make it live!

The key is to start simple (Option 1) and evolve as needed. The client-side approach can handle most use cases and can be deployed immediately for free!