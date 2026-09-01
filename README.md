# skill-recomposer

This is my skill recomposer. SWE is the first skill I’m putting through it.

Bassically, I am trying to come up with my own way of working with AI to build software. I think the key is to make AI break down its work into self containted modules that can be tested and verifed to work. This allows me to focus on reviewing what the software actaully does instead of just reiviewing code. My hope is that by making agents use TDD (test dirven development), that they will be able to self correct most of the errors I have seen while tryign to build with AI. 

So I am putting together my documents (skills, subagents) in this project to allow me to develop them. We then use the recompose skill to recompeose my docs for different platfroms, eg platforms that onlly support skills or just prompts. 

## Structure

My personal drafts can be found in the `src` folder. Here, I am putting my documents, orginally writen for cursor (since i know it best). 

Inside the src/swe direcroty is where the main action is. 

There is also src/review directory which offer some supporting documents. 

## Recompose

Ask Cursor, for example:

- `recompose SWE to skills only`
- `recompose to a Grok prompt`
- `recompose to Meta.ai`
- `recompose both`

That reads the canonical files and overwrites:


| Target      | Path                            |
| ----------- | ------------------------------- |
| Skills only | `dist/skills-only/swe/SKILL.md` |
| Portable    | `dist/prompts/portable.md`      |
| Grok        | `dist/prompts/grok.md`          |
| Meta.ai     | `dist/prompts/meta-ai.md`       |


Copy `dist/skills-only/swe/` into another project’s `.cursor/skills/` to use SWE without the agent. Copy a prompt file into Grok or Meta.ai as the system/first message.

## Edit, then recompose

Change SWE itself only in `src/swe/skills/swe/SKILL.md`. Then recompose. Do not hand-maintain `dist/`.