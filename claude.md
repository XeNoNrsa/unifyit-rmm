# CLAUDE.md - UnifyIT RMM Development Guide

This document guides Claude Code sessions for the UnifyIT RMM project. Reference this for all development decisions and implementation details.

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK START FOR EVERY SESSION:                              │
│                                                             │
│ 1. Read planning.md ✓                                       │
│ 2. Check Tasks.md ✓                                         │
│ 3. Enable MCPs: context7, magic, playwright, sequential ✓   │
│ 4. NEVER remove existing features - only add/improve ✓      │
│ 5. Mark tasks complete immediately ✓                        │
│ 6. Add a session summary to CLAUDE.md summarizing what we’ve done so far. ✓  │
   
                                                         │
│ Core Rule: NO MOCK DATA - PRODUCTION ONLY                  │
└─────────────────────────────────────────────────────────────┘
```

## 📌 SESSION START REQUIREMENTS

### At the Start of EVERY New Conversation:
1. **Read `planning.md`** - Get current project context and goals
2. **Check `Tasks.md`** - Review pending tasks before starting work
3. **Mark completed tasks** - Update immediately when tasks are done
4. **Add new tasks** - Document any newly discovered tasks

### Code Improvement Rules:
- **NEVER remove existing features or functions**
- **Only ADD or IMPROVE functionality**
- **Preserve all working code behavior**
- **Document why changes were made**

### Safe Code Improvement Checklist:
```typescript
// Before modifying ANY existing code:
const improvementChecklist = {
  1: "Does the current code work? If yes, proceed carefully",
  2: "Am I adding features or fixing bugs? Good!",
  3: "Am I removing functionality? STOP!",
  4: "Have I tested that existing features still work?",
  5: "Have I documented what I changed and why?"
};

// Example of GOOD improvement:
// OLD CODE:
function getUsers() {
  return db.query('SELECT * FROM users');
}

// IMPROVED CODE (adds pagination, keeps original functionality):
function getUsers(page = null, limit = null) {
  if (!page || !limit) {
    // Preserve original behavior when no params
    return db.query('SELECT * FROM users');
  }
  // Add new pagination feature
  const offset = (page - 1) * limit;
  return db.query('SELECT * FROM users LIMIT ? OFFSET ?', [limit, offset]);
}
```

### Required MCP Tools for ALL Conversations:
```yaml
MCP Tools to Enable:
  context7: "Real-time documentation injection for accurate code suggestions"
  magic: "Create crafted UI components inspired by top design engineers"
  playwright: "Control and interact with web browsers programmatically"
  sequential: "Structured problem-solving through step-by-step reasoning"
```

### Key Files to Reference:
```
📁 Project Root
├── 📄 planning.md      # Project goals and current sprint
├── 📄 Tasks.md         # Task list - UPDATE THIS!
├── 📄 CLAUDE.md        # This file - development guide
├── 📄 PRD.md           # Full product requirements
└── 📄 .env.example     # Environment variables template
```

### Task Priority System:
```yaml
When checking Tasks.md, prioritize in this order:
  P0 - CRITICAL: Security, payment, data loss prevention
  P1 - HIGH: Core features, performance issues
  P2 - MEDIUM: New features, improvements
  P3 - LOW: Nice-to-have, optimizations
  
Always complete:
  - P0 tasks immediately
  - Existing features before new ones
  - Bug fixes before enhancements
  - Tests for any new code
```

## 🚨 CRITICAL DEVELOPMENT DIRECTIVES

### Ultrathink Approach - MANDATORY
- **NO MOCK DATA** - Every feature must use real data
- **NO PLACEHOLDERS** - All functionality must be production-ready
- **NO HALLUCINATED FEATURES** - Only implement what can actually be built
- **COMPLETE IMPLEMENTATION** - Every feature must be fully functional

### Infrastructure Mandate
```bash
# BEFORE ANY DEVELOPMENT
# DELETE any existing infrastructure that doesn't align with these specs
# Review all existing code before implementation
# Remove deprecated or non-compliant components
```

### Technology Flexibility
- **Existing code with equivalent functionality is PREFERRED**
- Example: If current code has remote access != Guacamole but same features, KEEP IT
- Focus on FUNCTIONALITY, not specific tools
- Document any substitutions with justification

## 📋 PROJECT OVERVIEW

**Product**: UnifyIT RMM (Remote Monitoring and Management)  
**Architecture**: Multi-tenant SaaS with per-customer Docker deployments  
**Pricing**: $2/device (basic), $5/device (with AI)  
**Deployment**: 3 regions (NA, EU, ASIA)  
**Philosophy**: Build upon existing code - enhance rather than rebuild

## 🏗️ TECHNICAL ARCHITECTURE

### Core Stack
```yaml
Backend:
  - Language: Python (FastAPI) / Node.js acceptable
  - Databases: PostgreSQL, InfluxDB, MongoDB, Redis
  - Message Queue: Kafka, RabbitMQ
  - Container: Docker + Kubernetes
  - API: REST + GraphQL + WebSocket

Frontend:
  - Framework: Next.js 14+ (App Router)
  - UI: Material-UI + Tailwind CSS
  - State: Redux Toolkit + RTK Query
  - Language: TypeScript (mandatory)
  - SSR: Required (allocate resources!)

Agent:
  - Language: Rust (mandatory)
  - Size: <50MB memory, <1% CPU
  - Platforms: Windows, macOS, Linux, Mobile
  - ML Framework: Candle
  - Models: <100MB ONNX/TensorFlow Lite
```

### Per-Customer Deployment
```yaml
# EVERY customer gets isolated Docker stack
customer-stack:
  - Frontend (4 CPU, 4GB RAM minimum for SSR)
  - Backend API (8 CPU, 8GB RAM)
  - Agent Gateway
  - Worker Nodes
  - Redis Cache
  - PostgreSQL Schema (isolated)

DNS: {customer-id}.{region}.unifyit.io
```

## 🔑 KEY FEATURES TO IMPLEMENT

### 1. Device Management
```rust
// Rust Agent Core Structure
pub struct UnifyAgent {
    system_monitor: SystemMonitor,      // CPU, RAM, Disk, Network
    security_module: SecurityModule,    // Compliance, scanning
    automation_engine: AutomationEngine,// Script execution
    communication: CommunicationModule, // WebSocket/gRPC
    storage: StorageModule,            // Local caching
    plugin_system: PluginSystem,       // Extensibility
    ai_module: LocalAI,               // ML inference
}

// Performance Requirements
const MAX_CPU_USAGE: f32 = 0.01;     // 1%
const MAX_MEMORY_MB: u32 = 50;       // 50MB
const HEARTBEAT_INTERVAL: u32 = 60;  // seconds
```

### 2. Network Monitoring
```yaml
Discovery:
  - Automatic topology mapping (15-60 min initial)
  - Real-time updates (within minutes)
  - SNMP default attempts: "public", "private"
  - Proprietary inference when protocols unavailable
  - TrafficInsights: ML classification without DPI
  
Protocols:
  - SNMP: Port 161 (v1/v2c/v3)
  - Flow: Ports 2055,2056,4432,4739,6343,9995,9996
  - CDP/LLDP: 60-second intervals
  - Syslog: Port 514

Collector Limits:
  - Single: 2,000 devices, 9,000 interfaces
  - Shared: 200 sites, 1,500 devices, 8,000 interfaces
```

### 3. Remote Access (Native Integration)
```javascript
// Apache Guacamole OR equivalent - MUST be native
class RemoteAccess {
  // Native integration requirements:
  // - guacd proxy daemon integrated (not external)
  // - Custom HTML5 client in RMM dashboard
  // - Shared auth with RMM platform
  // - No external dependencies
  
  protocols = ['RDP', 'VNC', 'SSH', 'Telnet'];
  features = [
    'session-recording',
    'multi-user-sessions',
    'file-transfer',
    'chat-in-session',
    'screen-annotation',
    'mobile-support'
  ];
}
```

### 4. Backup System
```yaml
Storage Options:
  1. Wasabi (our storage): $8/TB
     - Auto-select region based on customer
     - No egress fees
  2. Customer cloud (AWS/Azure)
  3. Local storage (disk/NAS)

Features:
  - Customer-managed encryption keys
  - Compression + deduplication
  - CDP (Continuous Data Protection)
  - Instant file recovery
  - Cloud-to-cloud backup (O365, Google)
```

### 5. AI Implementation
```python
# LocalAI Backend (GPU clusters per region)
class LocalAI:
    def __init__(self):
        self.gpu_clusters = {
            'NA': {'nodes': 5, 'gpus': '20x A100'},
            'EU': {'nodes': 3, 'gpus': '12x A100'},
            'ASIA': {'nodes': 3, 'gpus': '12x A100'}
        }
    
    # Customer can also use their own API keys:
    supported_apis = [
        'OpenAI', 'Anthropic', 'Google Gemini',
        'DeepSeek', 'Ollama'
    ]

# Agent-side AI (Rust)
# Use Candle framework, models <100MB
# Local inference for anomaly detection
# Predictive maintenance
# Log analysis
```

### 6. Ticketing & PSA Integration
```javascript
// ALL PSA integrations MUST support:
const requiredFeatures = {
  ticketSync: 'bidirectional',
  assetSync: true,
  contactSync: true,
  automatedTickets: {
    creation: 'from alerts',
    closure: 'on resolution',
    timeEntry: 'for automation',
    customNotes: 'per automation'
  },
  customFieldMapping: true
};

// Supported PSAs
const psaList = [
  'HaloPSA', 'ConnectWise', 'Autotask',
  'DeskDay', 'Zendesk', 'Zoho'
];

// 3CX Phone Integration
const phoneFeatures = {
  autoTicketCreation: true,
  callLogging: true,
  screenPop: true,
  voicemailTranscription: true
};
```

## 🔌 INTEGRATION SPECIFICATIONS

### Security/EDR (All require bidirectional sync)
```yaml
Platforms:
  - SentinelOne, CrowdStrike, Bitdefender
  - ESET, Webroot, Checkpoint, WatchGuard
  - Fortinet, Sophos, Trend Micro

Requirements:
  - Silent deployment
  - Central alerts
  - Policy sync
  - Auto-remediation
  - Compliance reports
```

### Cloud Platforms
```typescript
// Office 365 / Google Workspace
interface CloudIntegration {
  userManagement: {
    create, modify, disable, delete,
    passwordReset, mfaConfig, licenses
  };
  
  automation: {
    offboarding: 'full workflow',
    onboarding: 'templated',
    licenseOptimization: 'ML-based'
  };
  
  security: {
    alerts: 'real-time',
    monitoring: 'continuous',
    governmentBackupRequests: 'special handling'
  };
}
```

## 🚀 DEPLOYMENT SPECIFICATIONS

### Customer Onboarding Flow
```python
async def onboard_customer(signup_data):
    # 1. Payment MUST be verified first
    if not payment_verified:
        return redirect_to_billing()  # NO ACCESS without payment
    
    # 2. Customer selects region (NA/EU/ASIA)
    region = signup_data.region
    
    # 3. Auto-provision infrastructure
    customer_dns = f"{customer_id}.{region.lower()}.unifyit.io"
    
    # 4. Deploy Docker stack
    deploy_customer_stack(customer_id, region)
    
    # 5. Create hidden super admin
    super_admin = {
        'username': 'unifyit_admin',
        'password': generate_secure_password(128),  # 128 chars
        'hidden_from_ui': True
    }
```

### Resource Scaling
```yaml
Scaling Rules:
  CPU > 70%: Scale up 50%
  Memory > 80%: Scale up 25%
  CPU < 30% for 5min: Scale down 25%
  Safety Buffer: Always keep 20% free
  
SSR Requirements:
  Frontend Minimum: 2 CPU, 2GB RAM
  Frontend Recommended: 4 CPU, 4GB RAM
  Note: SSR requires adequate resources!
```

## 📝 CODE PATTERNS & STANDARDS

### API Design
```typescript
// All APIs must follow these patterns
interface APIStandards {
  rest: {
    versioning: '/v1, /v2',
    pagination: 'required',
    filtering: 'standard params',
    response: 'consistent format'
  };
  
  graphql: {
    depthLimit: 10,
    costAnalysis: true,
    subscriptions: 'WebSocket'
  };
  
  errors: {
    format: 'RFC 7807',
    codes: 'standardized',
    messages: 'user-friendly'
  };
}
```

### Database Patterns
```sql
-- Multi-tenancy via schemas
CREATE SCHEMA customer_{id};

-- Row-level security
CREATE POLICY tenant_isolation ON table_name
  USING (tenant_id = current_tenant_id());

-- Audit everything
CREATE TRIGGER audit_trigger
  AFTER INSERT OR UPDATE OR DELETE ON table_name
  FOR EACH ROW EXECUTE FUNCTION audit_changes();
```

### Security Requirements
```yaml
All Code Must:
  - Use parameterized queries (no SQL injection)
  - Validate all inputs
  - Sanitize all outputs
  - Implement rate limiting
  - Use secure session management
  - Follow OWASP guidelines
  - Pass security scanning
```

## ⚡ PERFORMANCE TARGETS

```yaml
Agent:
  - CPU: <1% normal operation
  - Memory: <50MB
  - Network: <1KB/s average
  - Startup: <10 seconds

Platform:
  - API Response: <100ms (p95)
  - Dashboard Load: <2 seconds
  - Alert Processing: <5 seconds
  - Deployment Time: <5 minutes

Monitoring:
  - Discovery: 15-60 minutes
  - Topology Update: <5 minutes
  - Alert Generation: <30 seconds
```

## 🛡️ CRITICAL CONSTRAINTS

1. **Payment Enforcement**: NO access without payment - redirect to billing only
2. **Data Isolation**: Complete tenant separation, no data leakage
3. **Compliance**: GDPR, HIPAA, SOC 2 ready from day one
4. **Scalability**: Must support 1M+ devices globally
5. **Security**: Zero Trust architecture, E2E encryption
6. **Availability**: 99.99% uptime (52 min/year downtime)

## 📚 REFERENCE PRIORITIES

When implementing features, prioritize in this order:

1. **Core Platform** (authentication, multi-tenancy, billing)
2. **Agent Development** (Rust, cross-platform)
3. **Device Monitoring** (metrics, alerts)
4. **Remote Access** (native integration)
5. **PSA Integration** (ticketing, automation)
6. **Backup System** (multiple destinations)
7. **Network Monitoring** (discovery, topology)
8. **AI Features** (LocalAI, agent ML)
9. **Advanced Features** (migration, marketplace)
10. **Enterprise Features** (compliance, advanced security)

## 🚦 DEVELOPMENT CHECKLIST

Before ANY feature is considered complete:

- [ ] Real data implementation (no mocks)
- [ ] Performance meets specifications
- [ ] Security review passed
- [ ] Multi-tenant isolation verified
- [ ] API documented
- [ ] Tests written (>80% coverage)
- [ ] Error handling complete
- [ ] Audit logging implemented
- [ ] Scaling tested
- [ ] Documentation updated

## 🔧 DEVELOPMENT BEST PRACTICES

### Error Handling Without Breaking Features:
```javascript
// GOOD: Add error handling without changing behavior
async function processData(data) {
  try {
    // Original logic preserved
    const result = await existingLogic(data);
    
    // ADD validation without breaking
    if (!result || !result.id) {
      logger.warn('Unexpected result format', { data, result });
      // Still return result to maintain compatibility
    }
    
    return result;
  } catch (error) {
    // ADD error handling
    logger.error('processData failed', { error, data });
    
    // Maintain original behavior (let it throw)
    throw error;
  }
}
```

### Backward Compatibility Rules:
1. **API Changes**: Always add, never remove fields
2. **Database**: Only add columns/tables, use migrations
3. **Config**: New options must have defaults
4. **Features**: Hidden flags for gradual rollout
5. **Dependencies**: Test thoroughly before upgrading

## 💡 COMMON DECISIONS

**Q: Should I use technology X instead of specified Y?**  
A: YES, if X provides same/better functionality. Document why.

**Q: Can I simplify feature Z?**  
A: NO, all features must be implemented as specified.

**Q: Should I refactor existing working code?**  
A: Only if it doesn't meet performance/security requirements.

**Q: How to handle missing specifications?**  
A: Check existing code first, then ask for clarification.

**Q: Payment not working, can user access platform?**  
A: NEVER. Billing page only until payment confirmed.

## 🛠️ MCP TOOLS USAGE GUIDE

### context7
- **Purpose**: Real-time documentation injection
- **Use When**: Writing any code to ensure accuracy
- **Benefits**: Prevents outdated patterns, ensures best practices

### magic
- **Purpose**: Create premium UI components
- **Use When**: Building any user interface
- **Benefits**: Professional design quality, consistent aesthetics

### playwright
- **Purpose**: Browser automation and testing
- **Use When**: E2E testing, web scraping, automation
- **Benefits**: Reliable browser control, cross-browser testing

### sequential
- **Purpose**: Step-by-step problem solving
- **Use When**: Complex logic, debugging, architecture decisions
- **Benefits**: Clear reasoning, better solutions, documented thinking

## 🔍 TROUBLESHOOTING GUIDE

### Common Issues and Solutions:

**Issue: "Should I remove this deprecated code?"**
- ❌ NO! Add a deprecation warning instead
- ✅ Create new improved version alongside
- ✅ Plan migration path for future

**Issue: "This feature seems too complex"**
- ❌ Don't simplify by removing functionality
- ✅ Break into smaller functions
- ✅ Add clear documentation
- ✅ Improve error messages

**Issue: "Conflicting requirements"**
- Check existing code first
- Reference PRD.md for official spec
- Maintain backward compatibility
- Ask for clarification if needed

**Issue: "Performance problems"**
- Profile before optimizing
- Keep original logic as fallback
- Add feature flags for new optimizations
- Document performance improvements

---

## 🔄 SESSION REMINDERS

**Start of Session Checklist:**
- [ ] Read `planning.md` for project context
- [ ] Check `Tasks.md` for pending work
- [ ] Enable all required MCP tools (context7, magic, playwright, sequential)
- [ ] Review any recent code changes

**During Development:**
- [ ] Preserve all existing functionality
- [ ] Mark tasks complete in `Tasks.md` immediately
- [ ] Add new discovered tasks to `Tasks.md`
- [ ] Use MCP tools for better code quality

**End of Session:**
- [ ] Update `Tasks.md` with progress
- [ ] Document any blockers or decisions needed
- [ ] Commit with clear, descriptive messages

### Commit Message Format:
```bash
# Format: <type>(<scope>): <subject>

# Types:
feat: New feature
fix: Bug fix
improve: Enhancement to existing feature (NOT removal)
docs: Documentation only
test: Tests only
refactor: Code restructure (must preserve functionality)

# Examples:
feat(agent): Add anomaly detection using Candle framework
improve(api): Add pagination to getUsers while preserving original behavior
fix(billing): Enforce payment check before platform access
docs(setup): Add Docker SSR resource requirements

# NEVER use:
remove(feature): ❌ We don't remove features
breaking(api): ❌ We maintain backward compatibility
simplify(code): ❌ Often means removing functionality
```

---

**Remember**: This is a production system for MSPs managing critical infrastructure. Every decision impacts thousands of end users. Build accordingly.

## 📝 FINAL REMINDERS

**The Five Commandments of UnifyIT Development:**

1. **Thou shalt NOT remove existing features** - Only add or improve
2. **Thou shalt NOT use mock data** - Production-ready only
3. **Thou shalt NOT allow access without payment** - Billing page only
4. **Thou shalt ALWAYS check Tasks.md** - Before starting work
5. **Thou shalt ALWAYS use MCP tools** - For quality code

**Your Session Success Checklist:**
```
Before coding: Read planning.md ✓ Check Tasks.md ✓
During coding: Preserve features ✓ Use MCPs ✓
After coding:  Update Tasks.md ✓ Clear commits ✓
```

**When in doubt, remember:**
- Existing code is there for a reason
- Every feature in the PRD must be built
- MSPs depend on this for critical infrastructure
- Quality > Speed, always

---

*Last Updated: July 2025*  
*Version: 1.0*  
*For: UnifyIT RMM Development Team*