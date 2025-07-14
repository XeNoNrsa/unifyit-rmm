# UnifyIT RMM

**Remote Monitoring and Management Platform for Managed Service Providers**

UnifyIT RMM is a next-generation, multi-tenant SaaS platform designed to empower MSPs with enterprise-grade monitoring capabilities at an unbeatable price point of $2/device (basic) and $5/device (with AI features).

## 🎯 Vision

Democratize enterprise-grade RMM tools for MSPs of all sizes while maintaining the highest standards of security, performance, and reliability.

## 🚀 Key Features

- **Ultra-lightweight Rust agents** (<50MB memory, <1% CPU)
- **Multi-tenant architecture** with complete customer isolation
- **Global deployment** across 3 regions (NA, EU, ASIA)
- **Native remote access** integration
- **AI-powered insights** with LocalAI and customer API support
- **Comprehensive PSA integrations** (ConnectWise, Autotask, HaloPSA, etc.)
- **Enterprise backup solutions** with Wasabi storage
- **Zero Trust security** with end-to-end encryption

## 🏗️ Architecture

- **Frontend**: Next.js 14+ with TypeScript and SSR
- **Backend**: FastAPI (Python) with GraphQL and WebSocket support
- **Agent**: Rust with Candle ML framework
- **Databases**: PostgreSQL, InfluxDB, MongoDB, Redis
- **Container**: Docker + Kubernetes orchestration
- **Storage**: Wasabi (primary), customer cloud (secondary)

## 💰 Pricing

- **Basic**: $2/device/month - Core monitoring and management
- **AI Enhanced**: $5/device/month - Includes ML insights and automation

## 🔧 Development

This project follows strict development guidelines:
- **NO MOCK DATA** - Production-ready implementation only
- **Preserve existing features** - Only add or improve, never remove
- **Payment enforcement** - No access without verified billing
- **Multi-tenant isolation** - Complete data separation

See [CLAUDE.md](./CLAUDE.md) for detailed development guidelines.
See [PLANNING.md](./PLANNING.md) for project roadmap and architecture.
See [TASKS.md](./TASKS.md) for current development tasks.

## 📋 Getting Started

```bash
# Clone the repository
git clone https://github.com/XeNoNrsa/unifyit-rmm.git
cd unifyit-rmm

# Set up development environment
./scripts/setup-dev.sh

# Start local development
docker-compose up -d
```

## 🛡️ Security

- Zero Trust architecture
- End-to-end encryption
- GDPR, HIPAA, SOC 2 compliance ready
- Row-level security with tenant isolation
- Comprehensive audit logging

## 📊 Performance Targets

- **Agent**: <1% CPU, <50MB memory
- **API**: <100ms response time (p95)
- **Dashboard**: <2 second load time
- **Uptime**: 99.99% (52 minutes/year downtime)

## 🌍 Regions

- **North America**: US-East, US-West
- **Europe**: Frankfurt, Amsterdam  
- **Asia**: Singapore, Tokyo

## 📞 Support

For MSPs using UnifyIT RMM or development questions:
- Documentation: [docs.unifyit.io](https://docs.unifyit.io)
- Support: support@unifyit.io
- Community: [community.unifyit.io](https://community.unifyit.io)

## 📄 License

Proprietary - UnifyIT Technologies, Inc.

---

**Built for MSPs, by MSPs** 🚀