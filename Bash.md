---
id: bash
title: BASH
---

## create a file inside a folder
`mkdir B && touch B/README.md`

## Create a file inside a folder with content
```bash
mkdir B && touch B/README.md && cat > B/README.md << ENDOFFILE
---
id: main
title: RENAME HERE
---
ENDOFFILE
```

https://egghead.io/lessons/bash-course-overview-advanced-bash-automation-for-web-developers