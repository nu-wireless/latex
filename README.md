# Northeastern University Wireless Club (W1KBN) LaTeX Workshop

Welcome to the repository for the Northeastern University Wireless Club LaTeX Workshop.
This repository contains the slide deck and related materials used for our LaTeX Workshop.

---

## Workshop Slides

A precompiled PDF of the slide deck is available in the [Releases](https://github.com/melarb/nuwc-latex/releases) section for quick viewing.

---

## Compiling the Slides Yourself

### Intended Audience

This repository is primarily maintained by the Wireless Club Workshop Team.

The public can view the precompiled slide deck, but local compilation is primarily intended for officers. Successful compilation requires invoking the `--shell-escape` flag during build, as the source uses it to execute a Git command that retrieves the current commit hash for the footer.

To compile successfully, you must have a local clone of the repository that includes the `.git` directory. The build process depends on this metadata to parse the Git commit head through the `\iexec` command.

If you only download `main.tex` or a ZIP archive from GitHub, the `.git` directory will be missing and the version hash in the footer will not render correctly. In that case, you can still compile the slides by creating your own repository or removing the versioning command from the footer.

Officers with repository access can clone and compile directly. External users would need to fork or create their own repository to do the same.

---

### Requirements

To compile the slide deck locally, make sure you have:

* A LaTeX distribution such as **TeX Live**, **MiKTeX**, or **MacTeX**
* The `minted` package
  (requires Python and Pygments; compile with the `--shell-escape` flag)
* A LaTeX editor such as VS Code with the
  [LaTeX Workshop extension](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop), TeXstudio, or TeXShop

---

### Why Local Compilation Is Required

This presentation cannot be built online (such as on Overleaf) because it needs the `--shell-escape` flag, which allows LaTeX to execute external commands.
To my knolwedge, Overleaf does not support user projects that invoke shell commands during compilation.

The `--shell-escape` flag is needed because of the following command defined in the source:

```latex
\usepackage{iexec}
\newcommand{\gitAbbrevHash}{\iexec{git rev-parse --short HEAD}}
```

The `iexec` package runs the system command `git rev-parse --short HEAD` to automatically insert the current commit hash into the footer.
This requires both Git and a repository clone with its `.git` directory intact.
If you download only `main.tex` without the `.git` folder, the command will not return anything, and the “Version” field in the footer will be blank.

---

### How to Compile

1. Clone the repository

2. Compile using `pdflatex` (or any compiler) with the shell escape flag

   ```bash
   pdflatex --shell-escape main.tex
   ```

   If you are using VS Code with LaTeX Workshop, make sure the `--shell-escape` flag is included in your build recipe.

   I personally have only ever compiled this slide deck with pdflatex, so I cannot speak for lualatex or any otehr compilers.

3. **Compile twice**

   The slide deck uses the [`lastpage`](https://ctan.org/pkg/lastpage) package to display the total number of pages in the footer:

   ```latex
   \usepackage{lastpage}
   Version: \gitAbbrevHash{} \hspace{1em} \insertpagenumber/\pageref{LastPage}
   ```

   LaTeX requires two compilation passes to correctly resolve cross-references such as `\pageref{LastPage}` so it can display the total number of pages in the footer:

   * The **first pass** writes the page reference information to an auxiliary file (`.aux`), resulting in “??” placeholders.
   * The **second pass** reads this data and replaces the placeholders with the actual page count.

The frame count was previously used, but since introducing overlays into the slide deck (\onslide, \only) now the page numbers are used and introuced extra comoplexity. 

---

## Repository Structure

```
nuwc-latex/
├── main.tex         # Main Beamer slide deck source file
├── .gitignore       # Git ignore file
├── LICENSE          # License file
└── README.md        # This README file
```

---

## Troubleshooting

* **Blank version field in footer:** You are compiling outside a Git repository or missing the `.git` directory. You eitehr need to be an officer with push access or create your own Git repo.
* **Error's mentioning shell escape** Re-run compilation with the `--shell-escape` flag.
* **Page number shows “??”:** Run LaTeX a second time to resolve references.

---

## Questions

If you have any questions or feedback, please reach out:

* **Workshop Email:** [workshops@nuwireless.org](mailto:workshops@nuwireless.org)
* **Club Website:** [https://nuwireless.org](https://nuwireless.org)

---

## Design and Contributions

All workshop materials and repository design were created by [**Muhammad Elarbi**](https://melarbi.com) using LaTeX Beamer.
For more information, contact: [elarbi.m@northeastern.edu](mailto:elarbi.m@northeastern.edu)

This workshop was revived after its last run in Fall 2020.
Special thanks to [Jack Leightcap](https://jack.leightcap.com/) (President '22) and Connor Northway (VP '22).

---

## License

This project is licensed under the
[**Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License**](https://creativecommons.org/licenses/by-nc-sa/4.0/). <img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1" alt=""> <img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1" alt=""> <img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/nc.svg?ref=chooser-v1" alt=""> <img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/sa.svg?ref=chooser-v1" alt="">

