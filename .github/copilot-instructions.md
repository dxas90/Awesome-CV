# Awesome CV - AI Coding Agent Instructions

## Project Overview

Awesome CV is a LaTeX-based CV/Resume/Cover Letter template system. The core is a custom LaTeX class file ([awesome-cv.cls](../awesome-cv.cls)) that defines semantic markup for professional documents.

**Architecture**: The template uses a modular structure where:
- [awesome-cv.cls](../awesome-cv.cls) (863 lines) defines all styling, commands, and environments
- Main documents ([examples/resume.tex](../examples/resume.tex), [examples/cv.tex](../examples/cv.tex), [examples/coverletter.tex](../examples/coverletter.tex)) configure header/footer and `\input` content sections
- Content sections live in [examples/resume/](../examples/resume/) and [examples/cv/](../examples/cv/) directories as separate `.tex` files

## Build System

**Primary build command**: `make` (uses lualatex by default)
- Targets: `resume.pdf`, `cv.pdf`, `coverletter.pdf`, or `make examples` for all
- The [Makefile](../Makefile) uses `lualatex` as the compiler (stored in `CC` variable)
- Alternative: `xelatex {filename}.tex` (documented in README, compatible but not default)
- Docker option: `docker run --rm --user $(id -u):$(id -g) -i -w "/doc" -v "$PWD":/doc texlive/texlive:latest make`

**TeX Engine Requirement**: Must use XeLaTeX or LuaLaTeX (not pdfLaTeX) due to fontspec package dependency for modern font handling.

## Custom LaTeX Commands & Environments

### Personal Information Commands
Defined in [awesome-cv.cls](../awesome-cv.cls):
- `\name{First}{Last}`, `\position{title}`, `\address{location}`
- Social links: `\email{}`, `\homepage{}`, `\github{}`, `\linkedin{}`, `\twitter{}`, etc.
- `\quote{text}` - displays inspirational quote in header

### Document Structure Commands
- `\makecvheader[C|L|R]` - render header with alignment option
- `\makecvfooter{left}{center}{right}` - three-part footer
- `\cvsection{title}` - major section with colored divider line

### Content Entry Commands
Three main entry types defined in [awesome-cv.cls](../awesome-cv.cls):

1. **`\cventry{position}{organization}{location}{dates}{description}`**
   - Use inside `\begin{cventries}...\end{cventries}` environment
   - Description is typically a `\begin{cvitems}...\end{cvitems}` list
   - See [examples/resume/experience.tex](../examples/resume/experience.tex) for usage

2. **`\cvhonor{award}{title}{issuer}{date}`**
   - Use inside `\begin{cvhonors}...\end{cvhonors}` environment
   - For awards, certifications, achievements

3. **`\cvskill{category}{skills}`**
   - Use inside `\begin{cvskills}...\end{cvskills}` environment
   - For technical skills categorization

### Styling Commands
Font styles defined in [awesome-cv.cls](../awesome-cv.cls):
- Color scheme: `\colorlet{awesome}{awesome-red}` (options: awesome-emerald, awesome-skyblue, awesome-red, awesome-pink, awesome-orange, awesome-nephritis, awesome-concrete, awesome-darknight)
- Toggle color highlighting: `\setbool{acvSectionColorHighlight}{true|false}`
- Fonts: Source Sans 3 (main), Roboto, FontAwesome 6 (icons)

## File Organization Patterns

### Main Document Structure
```tex
\documentclass[11pt, a4paper]{awesome-cv}
\geometry{left=1.4cm, top=.8cm, right=1.4cm, bottom=1.8cm, footskip=.5cm}
% ... personal info commands ...
\begin{document}
\makecvheader[C]
\makecvfooter{\today}{Name~~~·~~~Document Type}{\thepage}
\input{resume/section1.tex}
\input{resume/section2.tex}
\end{document}
```

### Section Files
Each section file ([examples/resume/*.tex](../examples/resume/)) follows this pattern:
```tex
\cvsection{Section Title}
\begin{cventries}  % or cvhonors, cvskills
  \cventry{...}{...}{...}{...}{
    \begin{cvitems}
      \item {Description point}
    \end{cvitems}
  }
\end{cventries}
```

## Common Editing Patterns

### Adding Work Experience
Edit [examples/resume/experience.tex](../examples/resume/experience.tex) or [examples/cv/experience.tex](../examples/cv/experience.tex). Add new `\cventry` within the `cventries` environment. Order matters (most recent first).

### Enabling/Disabling Sections
In main document ([examples/resume.tex](../examples/resume.tex)), comment/uncomment `\input` lines to include/exclude sections.

### Customizing Colors
Change in main document: `\colorlet{awesome}{awesome-skyblue}` or define custom: `\definecolor{awesome}{HTML}{3E6D9C}`

### Modifying Fonts
Edit [awesome-cv.cls](../awesome-cv.cls). Current fonts: Source Sans 3, Roboto. Must be available in system or TeX distribution.

## Key Dependencies

- **LaTeX Distribution**: TeX Live (full) recommended, provides all required packages
- **Required Packages**: fontspec, unicode-math, fontawesome6, geometry, fancyhdr, xcolor, etoolbox, setspace, xifthen, xstring
- **Fonts**: Source Sans 3, Roboto, FontAwesome 6 (must be available to XeLaTeX/LuaLaTeX)

## CV vs Resume vs Cover Letter

- **Resume** ([examples/resume.tex](../examples/resume.tex)): Concise, 1-2 pages, summary section, centered header by default
- **CV** ([examples/cv.tex](../examples/cv.tex)): Comprehensive, multi-page, includes all experiences, left-aligned header by default
- **Cover Letter** ([examples/coverletter.tex](../examples/coverletter.tex)): Uses different environment (`cvletter`), includes recipient info, letter sections

Both CV and resume share the same section files structure but CV typically includes more sections.

## Critical Warnings

1. **Do NOT modify [awesome-cv.cls](../awesome-cv.cls) unless specifically asked** - it defines the entire template system
2. **Maintain .cls consistency**: [awesome-cv.cls](../awesome-cv.cls) in root must match [examples/awesome-cv.cls](../examples/awesome-cv.cls)
3. **TeX compilation**: Always use XeLaTeX or LuaLaTeX, never pdfLaTeX
4. **Character encoding**: Files must be UTF-8 (note BOM in file headers: `%!TEX encoding = UTF-8 Unicode`)
5. **Date formats**: Use human-readable format (e.g., "Mar. 2022 - Feb. 2025") not ISO dates
