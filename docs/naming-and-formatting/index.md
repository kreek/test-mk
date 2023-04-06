# Naming & Formatting

!!! success "Guidance"
    - Fields **should** use camelCase.
    - Acronyms **should** be camelCase rather than uppercase.
    - Providers **should** avoid abbreviations.
    - Booleans **should** be prefixed with an auxiliary verb (such as is, has, or can).
    - Enums **should** be UPPER_CASE strings with underscores in place of spaces.

## Case
Lighthouse recommends using camelCase rather than snake_case or kebab-case for JSON field names. Providers can use any programming language for their applications, including C-style languages (Ruby, Python, etc.) whose style guides/communities have decided on snake case for variable and field names. That said, we need to pick one case, and JavaScript is the dominant language for web clients/API consumers (and the J in JSON), and it uses came case.

## Acronyms
Acronyms are usually written in uppercase, which can confuse consumers reading field names. Most programming languages reserve uppercase for constants. Also, with camel case field names, it's hard to tell where the acronym ends and the next part of the variable begins, e.g., `BIRLSId` vs. `birlsId`.

## Abbreviations
For clarity, it's better to spell a word completely rather than use an abbreviation, especially a non-standard one. Modern editors and IDEs can autocomplete variable names, so abbreviations no longer save keystrokes. The VA has complicated terminology; future developers or your future self will appreciate the lack of ambiguity.

## Booleans
JSON is not typed, so prefixing boolean fields with an auxiliary verb (such as is, has, or can) marks it as a boolean field. This is also closer to natural language, e.g., run this code if the user is a veteran becomes `if (isVeteran) {}`.

