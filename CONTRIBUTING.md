# How to contribute?

To contribute to the core project or to BioTracs applications, please refer to following rules. 
Thanks :-).

## Coding conventions

Coding conventions are good coding practices that developers must follow to share their works. As BioTracs is based on the PRISM (Process-Resource Interfacing SysteM) architecture, it is not language specific.
It can therefore be implemented in any other language. Each language-specific implementation of BioTracs must therefore adopt language-specific coding conventions.


Above all language-specific conventions, the developer is *strongly* invited to carefully read the book *Clean Code: A Handbook of Agile Software Craftsmanship* of Robert C. Martin to adopt professional software development attitudes.
* https://fr.wikipedia.org/wiki/Robert_C._Martin
* https://www.pdfdrive.com/clean-code-e38664751.html 
* https://www.investigatii.md/uploads/resurse/Clean_Code.pdf

Other readings related to agile test-driven and behavior-driven developments (TDD/BDD):
* https://en.wikipedia.org/wiki/Test-driven_development


These coding conventions are fundamental to capture the state-of-mind behind PRISM and BioTracs-related implementations.

### Matlab coding conventions

The minimal structure a Matlab BioTracs application is:

* `+biotracs/`
  Contains the matlab library
* `assets/`
  Contains web library (css, js, ...). This folder is automatically copied in the view directories when HTML views are generated
* `backcomp/`
  Contains backward compatibility files (e.g. R2017a, R2017b, ...). This folder is automatically detected and loaded depending on the current version of Matlab. Please add here required .m files or packages if you use an unsupported version of Matlab.
* `externs/`
  Contains extern library on which the module depends. This folder is automatically detected and loaded.
* `tests/`
  Contains unit testing scripts
* `package.json`
  Contains the metadata of the package (i.e. application). Please see below. 
  The minimal informations are the dependencies. Recommended supplementary information are the name, version and description of the package.
* `CHANGELOG`
* `CONTRIBUTING.md`
* `DESCRIPTION.md`
* `LICENSE`
* `README.md`

The official coding convention adopted is the same as adopted by The MathWorks (R). Please see MATLAB Style Guidelines 2.0 from R. Johnson.
* https://fr.mathworks.com/matlabcentral/fileexchange/46056-matlab-style-guidelines-2-0.
* http://www.datatool.com/downloads/MatlabStyle2%20book.pdf

### Python coding conventions

The minimal structure a Python BioTracs application is:

* `biotracs/`
  The BioTracs python module
* `assets/`
  Contains web library (css, js, ...). This folder is automatically copied in the view directories when HTML views are generated
* `backcomp/`
  Contains backward compatibility files (e.g. 3.5, 3.4) automatically detected and loaded depending on the current version of Python. Please add here required .py files or modules if you use an unsupported version of Python.
* `externs/`
  Contains extern library on which the module depends. This folder is automatically detected and loaded.
* `tests/`
  Contains unit testing scripts
* `package.json`
  Contains the metadata of the package (i.e. application). Please see below. 
  The minimal informations are the dependencies. Recommended supplementary information are the name, version and description of the package.
* `CHANGELOG`
* `CONTRIBUTING.md`
* `DESCRIPTION.md`
* `LICENSE`
* `README.md`

### File package.json

The package.json file describes the *direct* BioTracs dependencies of the current package. 
When loading a package, its `package.json` file is automatically parsed and all its dependencies are also loaded. 

For example, consider a BioTracs application (called `biotracs-m-app-red`) with the following `package.json` file:


```json
{
	"name"			: "biotracs-m-app-red",
	"description"	: "My red biotracs application",
	"version"		: "0.1",
	"dependencies"	: [
		"biotracs-m-app-blue",
		"biotracs-m-app-green"
	],
}
```

Suppose that for the BioTracs application `biotracs-m-app-blue` we have the following `package.json` file;
 
```json
{
	"name"			: "biotracs-m-app-blue",
	"description"	: "My blue biotracs application",
	"version"		: "0.1",
	"dependencies"	: [
		"biotracs-m-app-purple",
		"biotracs-m-app-black"
	],
}
```

Then, the packages `biotracs-m-app1-purple` and `biotracs-m-app-black` will automatically be loaded when loading the package `biotracs-m-app-red`. 
This hierarchical loading of packages simplify the implementation of applications. Indeed, only *direct* dependencies are required. Also, when a dependency is provided several times, it is loaded only once. Cyclic dependencies are allowed.


## Git versionning rules

This section describes the practices rules to fork and version BioTracs applications. It is based on standard practices as explained at https://nvie.com/posts/a-successful-git-branching-model/

Please also refer to https://nvie.com/posts/a-successful-git-branching-model/

### Branch structures

* master branch
    * `master-{tag_number}`
    * `master-{tag_number}`
    * `...`
    * `master-{tag_number}`
* develop branch
    * `feature-{branch_name}`
    * `feature-{branch_name}`
    * `...`
    * `feature-{branch_name}`
* release branch
    * `release-{tag_number}`
    * `release-{tag_number}`
    * `...`
    * `release-{tag_number}`
* hotfix branch
    * `fix-{tag_number}`
    * `fix-{tag_number}`
    * `...`
    * `fix-{tag_number}`

### Software version numbering convention

Version numbering convention is only for the releases, hotfixes (e.g. `release-x.y.z`, `hotfix-x.y.z`). It is then propagated to the master branch (e.g. `master-x.y.z`). The numbering is performed using `tags`.

BioTracs applications versioning follows [Semantic Versioning](https://semver.org/) guidelines

* numbering: `x[.y[.z]]` = `marjor[.minor[.patch]]`
** required: `major` version when you make incompatible API changes
** optional: `minor` version when you add functionality in a backwards-compatible manner
** optional: `patch` version when you make backwards-compatible bug fixes

No tag is required on the `feature` and `develop` branches.

### Git commit/push/merge conventions

Developers are only allowed to commit/push on the `develop`, `feature` branches. Commits on these branches must be named unambiguously and concisely commented with a short text describing the changes:
* `feature:description of the feature`
* `bugfix:bug to be fixed`
* `fix:bug to be fixed`

After fixing a bug or developing a new feature, changes are merged with the `develop` branch before being merged with the `release` branch. Only the `release` branch must be merged with the `master` branch.

### What are the branches use for?

#### Master branch 

No development is allowed on the master branch. This branch is not shared with external users (the release branch is devoted for that purpose).
 
#### Develop branch

Only for development. Features branches are used to develop new features. After a feature development is completed, it is merged back with the `develop` branch. Feature branches must not be merged with the `release` branch
 
#### Release branch

The `release` branch is created from the develop branch. It is only used to share the code with external users in production mode. External user projects must used codes in the `release` branch at a well-defined version given by a commit `tag`.
 
* No development is allowed on this branch.
* Only pull operations are allowed (for user projects) 

#### Hotfix branch

Only used to fix particular bugs in the `release` branch. Hot-fixes are propagated to the `develop` branch and then to the `release` branch.

Thank you for your contribution :-).