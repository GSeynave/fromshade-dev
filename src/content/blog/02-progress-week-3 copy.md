---
title: 'Elevate - Weeks 3: The step back'
type: 'Progress'
description: 'How early morning coding session added more than 10h/week wihtout burning out.'
pubDate: 'Jan 18 2025'
tags: ['journal','Kotlin','DSA','Architecture', 'Data Structure','SaaS']
enabled: false
---
## Overall progress

*Description of progress*

## The data

**Week 3 (Jan 5-11):**
- DSA : 3.5h
- SaaS : 10h

**Total time spent on project in 2 weeks: xxh**

## Week planning
*DSA*:
- [x] Finish encode and decode string !
- [ ] Start the next problem- WIP
 
*SaaS*:
- [ ] Finish kotlin migration and have a working (runnable) server.
- [ ] Clarify endpoint's needed for frontend.
- [ ] Fully clean the code base *Here the important thing is to understand every single line of code purpose ! (not algorithm wise but technically wise)*
- [ ] unit test + hurl test

## Key Takeaways

- Commute time is energy draining a lot. Reducing productivity on side project on work from office days.
- Took a step back on the current situation → AI generated code reliquary, data model not clean, not DDD compliant(cross module).
- Rework the project structure to be more DDD / Hexagonal compliant
  - Port for repository (no coupling with infrastructure layer)
  - Use case service layer
  - Api layer to expose data to other module
  - CQS pattern → separate read and write models (command / query)
- Focus on user / tenant and todo mainly (put habits and accounting aside) to complete a module fully / cleanly

## Next Goals
