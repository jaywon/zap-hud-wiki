# zap-hud-wiki
Wiki documentation site for the OWASP ZAP HUD project

### Dependencies
- `python`
- `mkdocs`
- `mkdocs-material`


### Installation
The primary dependencies are `python` and `mkdocs`. 

Assuming `python` is already installed:

`pip install mkdocs mkdocs-material`

**RECOMMENDATION:** Run the project in `virtualenv` to localize dependencies and reduce conflicts across operating systems and versions of Python.

### Development/Contribution
##### To contribute to the wiki:
1. Fork this repository
1. `git clone https://github.com/your-user/zap-hud-wiki.git`
1. Submit pull requests back to this repository

##### Adding to the wiki:

See the official [Mkdocs website](https://www.mkdocs.org/) documentation for information on how to add/edit pages.

##### Run Local Dev Server
1. `cd /path/to/zap-hud-wiki`
1. `mkdocs serve`
1. Site will now be accessible at: `http://127.0.0.1:8000/`

##### Build new release
1. `mkdocs build`
2. Generated site files will now be accessible in the `site` directory for deployment