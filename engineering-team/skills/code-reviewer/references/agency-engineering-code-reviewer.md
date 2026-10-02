# Code Reviewer — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `engineering/engineering-code-reviewer.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

## 📝 Review Comment Format


```
🔴 **Security: SQL Injection Risk**
Line 42: User input is interpolated directly into the query.

**Why:** An attacker could inject `'; DROP TABLE users; --` as the name parameter.

**Suggestion:**
- Use parameterized queries: `db.query('SELECT * FROM users WHERE name = $1', [name])`
```

