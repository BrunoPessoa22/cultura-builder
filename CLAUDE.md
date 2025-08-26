# Brazilian Fan Token Dashboard - Claude Code Context

## Project Overview
Upgrading Brazilian Fan Token dashboard to match DeFi Station Agent quality with sophisticated dark theme, real-time blockchain integration, and AI-powered insights.

**Target Teams**: Flamengo (MENGO), Corinthians (SCCP), Palmeiras (VERDAO), Fluminense (FLU), Bahia, MIBR, Vasco da Gama (VASCO)

## Technology Stack
- **Frontend**: Next.js 14, TypeScript, Tailwind CSS, Framer Motion
- **Backend**: FastAPI with WebSocket support
- **Blockchain**: Chiliz Chain (ChainID: 88888)
- **Charts**: Chart.js with real-time updates, D3.js for advanced visualizations
- **State**: Zustand + React Query + WebSocket subscriptions
- **Styling**: Glass-morphism, dark theme, team-specific accents
- **Cache**: Multi-layer (Memory + Redis + Database)

## Multi-Agent Architecture
- **Lead Dashboard Agent**: Orchestrates all operations and coordinates sub-agents
- **UI Agent**: Component generation, styling, animations
- **Blockchain Agent**: Chiliz Chain interactions, smart contract monitoring
- **Data Agent**: Real-time price feeds, WebSocket management
- **Analytics Agent**: Trading patterns, AI insights, predictions
- **Error Agent**: Error boundaries, fallback strategies
- **Performance Agent**: Caching, optimization, monitoring

## Implementation Priority
1. **Phase 1**: Dark theme with glass-morphism effects
2. **Phase 2**: Real-time WebSocket price updates
3. **Phase 3**: AI-powered trading suggestions
4. **Phase 4**: Social sentiment integration
5. **Phase 5**: Match correlation features
6. **Phase 6**: Gamification elements

## Chiliz Chain Integration
```javascript
// Token Contracts on Chiliz Mainnet
const TOKENS = {
  MENGO: '0xd1723eb9e7c6ee7c7e2d421b2758dc0f2166eddc',
  SCCP: '0x20BFeab58f8bE903753d037Ba7e307fc77c97388',
  VERDAO: '0x971364Ec452958d4D65Ba8D508FAa226d7117279',
  FLU: '0x86930777d43605C40bA786F7802778ff5413eFaB',
  BAHIA: '0xE92e152fC0ff1368739670a5175175154Ceeef42',
  MIBR: '0xa8206Af1e6a0289156d45B9d60e5bbD5d1fCf68d',
  VASCO: '0x689dee82d191dac845599f46393c2efe4b5896a2'
};

// RPC Endpoints
const RPC_ENDPOINTS = [
  'https://rpc.ankr.com/chiliz',
  'https://chiliz.publicnode.com',
  'wss://chiliz.publicnode.com' // WebSocket
];
```

## Design System
- **Background**: #0a0a0a to #1a1a1a gradient
- **Cards**: Glass-morphism with backdrop-blur-xl
- **Accent Colors**: Team-specific dynamic theming
- **Success**: #00ff88 (Brazilian green)
- **Warning**: #fbbf24 (Brazilian yellow)
- **Animations**: Framer Motion with spring physics
- **Grid**: 8px spacing system
- **Typography**: Inter/Poppins with responsive scale

## Component Structure
```
components/
├── cards/
│   ├── FanTokenCard.tsx (glass-morphism, live prices)
│   ├── StatsCard.tsx (animated counters)
│   └── TeamComparisonCard.tsx
├── charts/
│   ├── TradingVolumeChart.tsx (real-time updates)
│   ├── TeamRadarChart.tsx (comparison)
│   └── PriceSparkline.tsx
├── layouts/
│   ├── DashboardLayout.tsx (responsive grid)
│   └── Sidebar.tsx (collapsible)
├── modals/
│   ├── TradingModal.tsx
│   └── WalletModal.tsx
└── shared/
    ├── ThemeProvider.tsx
    └── WebSocketProvider.tsx
```

## Key Features Implementation

### Real-time WebSocket Connection
- Subscribe to all team token price feeds
- Handle reconnection with exponential backoff
- Update UI with smooth animations
- Cache data for offline functionality

### AI Trading Insights
- Multi-agent analysis system
- Sentiment analysis from social media
- Technical indicators calculation
- Match correlation patterns
- Risk assessment scoring

### Brazilian Cultural Integration
- Portuguese as primary language
- Team rivalry features
- Carnival season themes
- Match day celebrations
- Torcida loyalty program

## Performance Requirements
- Initial load: < 3 seconds
- Price updates: < 500ms latency
- Smooth 60fps animations
- Mobile responsive
- Offline capability

## Development Workflow
1. Use 3 Amigo Agents pattern for requirements
2. Deploy specialized agents for each feature
3. Implement with TypeScript for type safety
4. Test with real Chiliz Chain data
5. Optimize bundle size and performance
6. Deploy with CI/CD pipeline

## Testing Strategy
- Unit tests for utilities
- Integration tests for blockchain
- E2E tests for critical paths
- Performance monitoring
- Real device testing

## Deployment
- Frontend: Vercel with edge functions
- Backend: AWS Lambda or Railway
- Database: PostgreSQL with TimescaleDB
- Cache: Redis with Upstash
- CDN: CloudFlare for global distribution

## Success Metrics
- 10K+ daily active users
- < 3s page load time
- 99.9% uptime
- < R$20 user acquisition cost
- 70% trading signal accuracy

This context file serves as the source of truth for all Claude Code agents working on the Brazilian Fan Token Dashboard upgrade.