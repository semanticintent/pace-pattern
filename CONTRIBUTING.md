# Contributing to PACE

Thank you for your interest in contributing to the PACE Pattern! This document explains how you can participate in this open research and design initiative.

---

## Ways to Contribute

### 1. Implement PACE in Your Project

The best contribution is **using PACE** and sharing what you learn.

**If you build a PACE implementation:**
1. Let us know! Open an issue with the `implementation` label
2. Share metrics (if you can)
3. Document what worked and what didn't
4. Consider writing a case study

**We'll feature quality implementations** in the main README and examples directory.

---

### 2. Share Research & Insights

PACE benefits from real-world data and academic research.

**Research contributions we value:**
- User studies comparing PACE vs. traditional UX
- Metrics from production PACE implementations
- Academic papers citing or extending PACE
- UX research on guide-first interfaces
- Accessibility studies
- Multi-language/cultural adaptation findings

**How to share:**
- Open an issue with the `research` label
- Include methodology, findings, and (if possible) data
- We may incorporate findings into documentation with attribution

---

### 3. Improve Documentation

Found something unclear? Help make it better.

**Documentation improvements:**
- Fix typos, broken links, formatting issues
- Add examples and clarifications
- Translate documentation (see localization section)
- Create diagrams, visuals, videos
- Write tutorials or guides

**How to contribute:**
1. Fork the repository
2. Make your changes in a branch
3. Submit a pull request
4. Describe what you improved and why

---

### 4. Submit Case Studies

Share your experience implementing PACE.

**What makes a good case study:**
- Specific implementation details
- Metrics and outcomes
- Challenges faced and solutions found
- Screenshots, code samples, or demos
- Lessons learned

**Format:**
```markdown
# [Project Name]: PACE Implementation Case Study

**Author:** [Your Name]
**Date:** [YYYY-MM-DD]
**Project:** [Brief description]
**URL:** [If public]

## Context
[What problem were you solving?]

## Implementation
[How did you implement PACE principles?]

## Results
[What happened? Metrics, user feedback, outcomes]

## Lessons Learned
[What worked? What didn't? What would you do differently?]
```

**Submit via:**
- Pull request to `examples/[project-name]/`
- Or open an issue with the `case-study` label

---

### 5. Extend the Framework

Propose extensions, variations, or refinements to PACE.

**Examples of extensions:**
- Industry-specific PACE patterns (healthcare, education, etc.)
- New interaction patterns
- Additional measurement frameworks
- Integration with other design systems
- Accessibility enhancements

**Proposal format:**
1. Open an issue with the `proposal` label
2. Describe the extension clearly
3. Explain the benefit
4. Provide examples or prototypes if possible
5. Discuss in comments before implementing

**Acceptance criteria:**
- Aligns with PACE principles
- Provides clear value
- Well-documented
- Includes examples

---

## Contribution Guidelines

### Pull Request Process

1. **Fork & Branch**
   ```bash
   git clone https://github.com/semanticintent/pace-pattern.git
   cd pace-pattern
   git checkout -b feature/your-contribution
   ```

2. **Make Changes**
   - Follow existing formatting
   - Use clear, concise language
   - Add examples where helpful
   - Test all links

3. **Commit**
   ```bash
   git commit -m "docs: improve executive summary section"
   ```
   Use conventional commit format:
   - `docs:` for documentation
   - `feat:` for new features
   - `fix:` for corrections
   - `research:` for research additions
   - `example:` for case studies

4. **Submit PR**
   - Clear title describing the change
   - Description explaining why the change is valuable
   - Reference any related issues

5. **Review**
   - Maintainers will review within 7 days
   - Address feedback in new commits
   - Once approved, we'll merge

---

### Documentation Style Guide

**Tone:**
- Clear, direct, professional
- Educational, not prescriptive
- Evidence-based when possible
- Encouraging experimentation

**Format:**
- Use markdown headers hierarchically
- Include code examples in appropriate language blocks
- Use tables for comparisons
- Add diagrams where they clarify (Mermaid or images)

**Structure:**
- Lead with the why, then the how
- Provide both principles and practical examples
- Include anti-patterns (what not to do)
- End with actionable takeaways

**Examples:**
✅ Good: "The Executive Summary Panel increases trust by making context visible."
❌ Bad: "You should always use an Executive Summary Panel."

---

## Localization

We welcome translations to make PACE globally accessible.

**Priority languages:**
- Spanish (Español)
- French (Français)
- German (Deutsch)
- Japanese (日本語)
- Mandarin Chinese (中文)

**How to translate:**
1. Create `translations/[lang-code]/` directory
2. Translate core documents:
   - README.md
   - PRINCIPLES.md
   - IMPLEMENTATION.md
3. Keep structure and links intact
4. Submit as PR with `translation` label

**Translation guidelines:**
- Maintain technical accuracy
- Adapt examples to local context when appropriate
- Keep PACE acronym in English, explain in local language
- Translate metaphors thoughtfully (cormorant may not be culturally relevant everywhere)

---

## Code of Conduct

### Our Commitment

PACE is an open research initiative. We're committed to:
- Respectful, constructive discussion
- Welcoming diverse perspectives
- Giving credit where due
- Collaborative improvement over competition

### Expected Behavior

- Be respectful and professional
- Provide constructive feedback
- Give credit and attribution
- Focus on ideas, not individuals
- Help newcomers

### Unacceptable Behavior

- Harassment or discrimination
- Personal attacks
- Plagiarism or false attribution
- Spam or self-promotion without value

**Enforcement:** Violations may result in removal of contributions or access to the project. Report issues to [contact info].

---

## Attribution

All contributors will be credited in:
- CHANGELOG.md for specific contributions
- README.md for significant contributions
- Individual documents for subject-matter contributions

**Attribution format:**
```markdown
**Contributors:**
- [Name] ([ORCID](link) if available) - [Contribution]
- [Name] - [Contribution]
```

---

## License

By contributing, you agree that your contributions will be licensed under the same [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) license as the project.

You retain copyright to your contributions but grant the project rights to use, modify, and distribute them under CC BY 4.0.

---

## Recognition Program

### PACE Certified Implementations

Projects that fully implement PACE principles may receive **PACE Certified** recognition:

**Criteria:**
- Implements all four PACE principles
- Provides metrics demonstrating effectiveness
- Shares learnings with community
- Maintains implementation quality over time

**Benefits:**
- Badge for your project
- Featured in PACE README
- Case study published
- Community showcase

**Apply:** Open an issue with the `certification` label and provide:
- Project URL
- Metrics data
- Implementation documentation
- Demo video (optional)

---

## Questions?

- **General questions:** Open a discussion in GitHub Discussions
- **Bug reports:** Open an issue with the `bug` label
- **Feature requests:** Open an issue with the `enhancement` label
- **Private inquiries:** [Contact email/form]

---

## Acknowledgments

PACE was created by **Michael Shatny** ([ORCID: 0009-0006-2011-3258](https://orcid.org/0009-0006-2011-3258)) as part of the Semantic Intent ecosystem.

Special thanks to:
- The cormorants at Mill Pond Park, Richmond Hill, Ontario (the real MVPs 🐦)
- Early MillPond users for feedback
- Claude (Anthropic) for making agentic UX possible
- The open source community for inspiration

---

*Part of the [PACE Pattern](https://github.com/semanticintent/pace-pattern) documentation.*
*Licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).*
