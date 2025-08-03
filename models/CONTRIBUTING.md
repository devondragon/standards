
# Contributing to the MACH Open Data Model (ODM)

> **This is a curated resource of production-proven patterns, not an open wiki**

The MACH Open Data Model is designed to capture real-world integration patterns that have been successfully implemented in production environments. We maintain high standards to ensure every contribution provides genuine value to the MACH community.

## 🎯 Before You Contribute

### Prerequisites
1. **Production Experience Required**: All patterns must be based on actual real world implementations
2. **Template Compliance**: Contributions must follow our templates exactly
3. **Vendor Neutrality**: Patterns must work across multiple vendors
4. **Community Discussion**: Major patterns should be discussed in the [MACH Alliance ODM Forum](https://community.machalliance.org/home/forum/boards/special-programs-ous?topicId=684876b4b4047405d2cd909e&topicName=Open%20Data%20Model) first

## 📊 What We Accept

### ✅ We Welcome
- **Entities**: Canonical data models proven in production
- **Recipes**: Integration patterns solving real business problems
- **Utility Objects**: Reusable components (Address, Money, etc.)
- **Improvements**: Enhancements to existing patterns based on implementation feedback

### ❌ We Don't Accept
- Theoretical or untested patterns
- Vendor-specific implementations (without abstraction)
- Duplicates of existing patterns
- Incomplete submissions
- Patterns that don't follow templates

## 🚦 Contribution Tiers

### Tier 1: Quick Fixes (Auto-approved)
- Typo corrections
- Broken link fixes
- Formatting improvements
- JSON syntax fixes

**Process**: Submit PR directly → Quick Review → Merged

### Tier 2: Pattern Contributions (Periodic Detailed Review)
- New entities or recipes
- Significant updates to existing patterns
- New utility objects

**Process**:
1. Ensure pattern is based on real world production scenarios
2. Follow template EXACTLY
3. Include real-world context
4. Submit PR with required information
5. Get 2+ community validations
6. Periodic batch review by maintainers

### Tier 3: Strategic Changes (Forum Discussion Required)
- New pattern categories
- Template modifications
- Architectural decisions
- Process changes

**Process**: Discuss in [ODM Forum](https://community.machalliance.org/home/forum/boards/special-programs-ous?topicId=684876b4b4047405d2cd909e&topicName=Open%20Data%20Model) → Maintainer decision → Implementation

## 📝 Contribution Process

### Step 1: Validate Your Contribution
Before starting, ensure you have:
- [ ] A pattern successfully running in production
- [ ] Clear business problem it solves
- [ ] Evidence it works across multiple vendors (or could)
- [ ] Time to follow our templates exactly

### Step 2: Choose the Right Template
- **Entities**: Use [master-entity-template.md](templates/master-entity-template.md)
- **Recipes**: Use [master-recipe-template.md](templates/master-recipe-template.md)
- **Read**: [Creating Canonical Data Models Guide](templates/creating-canonical-data-models.md)

### Step 3: Prepare Your Submission

#### For Entities:
```markdown
## Required Information
- Entity name and purpose
- All fields with descriptions and types
- JSON example with realistic data
- Extension patterns for flexibility
- Which existing entities it relates to
```

#### For Recipes:
```markdown
## Required Information
- Business problem being solved
- Systems/platforms involved
- Complete data flow
- Working code examples
- Known limitations
- Success metrics from production
```

### Step 4: Submit Your PR

Your PR description MUST include:
```markdown
## Production Use Case
Company/Project: [Where this was implemented]
Timeline: [When it was implemented]
Scale: [Approximate usage metrics]

## Problem Solved
[Specific business problem this addresses]

## Vendors/Platforms Tested
[List of MACH vendors this works with]

## Community Validation
[Tag 2+ community members who can validate this pattern]
```

### Step 5: Community Validation
- Your PR needs 2+ validations from community members
- Validators should confirm they've seen similar patterns work
- Maintainers review validated PRs monthly

## 🔍 Review Criteria

Maintainers evaluate contributions based on:

1. **Production Proven**: Is this from a real implementation?
2. **Vendor Neutral**: Can this work across multiple vendors?
3. **Well Documented**: Does it follow templates completely?
4. **Unique Value**: Does this add something new?
5. **Best Practice**: Is this a recommended approach?

## ⚡ Fast Track Options

Want your contribution reviewed faster?

1. **Perfect Template Compliance**: No formatting issues = faster review
2. **Community Pre-Review**: Get 5+ validations = priority review
3. **Reference Implementation**: Include working code = immediate attention
4. **Multiple Vendor Proof**: Show it works across 3+ vendors = fast track

## 🚨 Important Notes

### Maintainer Rights
The ODM maintainers reserve the right to:
- Edit contributions for consistency
- Reject without detailed explanation
- Request production evidence
- Sunset outdated patterns
- Prioritize reviews based on community value

### Your Rights
- Attribution for your contributions
- Ability to update your patterns based on new learnings
- Voice in forum discussions
- Recognition as ODM contributor

### Review Timeline
- **Tier 1**: Immediate (automated)
- **Tier 2**: Monthly batch reviews (first Monday of each month)
- **Tier 3**: As needed based on forum discussions

## 📞 Getting Help

1. **Technical Questions**: Open an [Issue](https://github.com/machalliance/standards/issues/new)
2. **Pattern Discussions**: [ODM Forum](https://community.machalliance.org/home/forum/boards/special-programs-ous?topicId=684876b4b4047405d2cd909e&topicName=Open%20Data%20Model)
3. **Template Help**: See [Creating Canonical Data Models](templates/creating-canonical-data-models.md)

## 📜 License

All contributions are released under [CC BY 4.0](../LICENSE). By contributing, you agree to this license and confirm you have the right to submit the patterns.

---

**Remember**: Quality over quantity. One well-documented, production-proven pattern is worth more than ten theoretical ones.
