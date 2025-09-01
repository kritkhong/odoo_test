# Claude Code - Project Context

## Current Project: Odoo Learning Path
**Status**: Learning Odoo v18 (16GB local installation via git clone)

## Odoo Learning Philosophy: Clean Architecture Approach
### Core Principle
"Odoo is a data platform, not a development framework"

**Responsibility Separation**:
- Odoo handles: Data storage, UI, standard workflows
- External services handle: Business logic, calculations, complex integrations
- Clean APIs connect the layers

## Architecture Philosophy

### The Clean Pattern (Always Follow)
```python
# Odoo: Minimal integration layer only
class SaleOrder(models.Model):
    _inherit = 'sale.order'
    
    # Storage fields (readonly)
    commission_amount = fields.Float(readonly=True)
    calculation_status = fields.Selection([...], readonly=True)
    last_calculated = fields.Datetime(readonly=True)
    
    # Simple trigger (no business logic)
    def trigger_commission_calculation(self):
        requests.post('/api/calculate-commission', json={'sale_id': self.id})

# FastAPI: All business logic here
@app.post("/calculate-commission")
async def calculate_commission(sale_data: SaleOrderData):
    # Complex calculations, ML, external APIs, etc.
    result = complex_business_logic(sale_data)
    # Update Odoo via API
    await update_odoo(sale_data.sale_id, result)
    return result
```

### ❌ Never Do This (Avoid at All Costs)
```python
# ❌ DON'T: Business logic in Odoo models
class SaleOrder(models.Model):
    _inherit = 'sale.order'
    
    def action_confirm(self):
        self._calculate_commission()  # ❌ Complex logic in Odoo
        self._send_notifications()    # ❌ External integrations in Odoo
        super().action_confirm()
    
    def _calculate_commission(self):
        # ❌ 100+ lines of business logic in model
        pass
```

## Learning Strategy
Phase 1: Foundation (Months 1-3)
Goal: Learn Odoo boundaries, not deep customization

✅ Learn: API usage (JSON-RPC), basic field additions, functional modules
✅ Practice: Build external FastAPI services that read/write Odoo data
❌ Avoid: Complex inheritance, computed fields, method overriding

Phase 2: Clean Integration (Months 2-4)
Goal: Perfect the external service pattern

✅ Build: Commission calculator, interest calculator as separate services
✅ Store: Only results in Odoo (readonly fields)
✅ Test: External services independently (fast, reliable unit tests)

Phase 3: Business Credentials (Months 4-6)
Goal: Get Odoo partner status via functional expertise

✅ Focus: Functional certifications (Sales, Accounting, Inventory)
✅ Position: As business process expert, not technical hacker
❌ Avoid: Deep technical development certifications

Pitfalls to Remember
Tutorial Seduction

When tutorials say: "Just add business logic to the model"
I respond: "Business logic belongs in external services"

Enterprise Pressure

When consultants say: "You need Enterprise or project will fail"
I respond: "Community first, migrate to Enterprise when specific features provide proven ROI"

Revenue Model Corruption

Avoid: Creating problems to solve later for recurring revenue
Follow: Build systems that work, charge subscription for ongoing value

Framework Worship

Remember: Odoo conventions don't override clean architecture principles
Priority: Long-term maintainability > short-term convenience

Business Model
Service Offerings

Community-first implementations (lower risk, prove value first)
Clean external service development (competitive advantage)
Subscription-based system evolution (ongoing value delivery)
Easy Enterprise migration (when features actually needed)

Value Proposition
"We build Odoo systems that stay maintainable and upgradeable by keeping business logic in external services where it belongs."
Key Mantras
Technical

"If it's complex, it's external"
"Odoo stores data, services run logic"
"Readonly fields are my friend"
"API calls, not inheritance"

Business

"Community first, Enterprise when proven"
"Client independence, not dependency"
"Subscription for value, not problems"
"Systems that improve over time"

Learning

"Functional expert, not technical hacker"
"Architecture principles over framework tricks"
"My Django pain taught me what not to do"

Safe Learning Sources
✅ Use These

Odoo API Documentation - Integration patterns
Functional training materials - Business process understanding
"Clean Architecture" by Robert Martin - Core principles
FastAPI documentation - Service architecture

❌ Filter These Heavily

Odoo development tutorials - Usually teach bad patterns
YouTube customization videos - Promote technical debt
Stack Overflow solutions - Often complex inheritance hacks
Community "best practices" - Usually framework-centric, not architecture-centric

Success Metrics
Month 3: Foundation Complete

✅ Built working external service integrated with Odoo
✅ Zero complex business logic in Odoo models
✅ Clean API boundaries established

Month 6: Business Ready

✅ Functional certification obtained
✅ Partner application submitted
✅ Portfolio of clean implementations

Month 12: Market Position

✅ Known for "systems that actually work and upgrade easily"
✅ Subscription revenue from ongoing value delivery
✅ Client referrals based on system quality

My Competitive Advantage
While others: Create technical debt through heavy customization
I deliver: Clean, maintainable, upgradeable systems
While others: Lock clients into maintenance nightmares
I deliver: Client independence with ongoing value partnership
While others: Fear Odoo upgrades due to broken customizations
I deliver: Easy upgrades because clean architecture
This approach creates sustainable competitive advantage in a market full of extractive consulting practices.

Remember: Trust architectural instincts over framework "expertise." The goal is building business solutions that happen to use Odoo, not becoming an "Odoo expert" who creates unmaintainable systems.