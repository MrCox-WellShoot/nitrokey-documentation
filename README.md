# Nitrokey's User Documentation

## Tips and Conventions

- See syntax examples in `syntax.rst` and live at https://docs.nitrokey.com/syntax.html 

- Write filenames in lower case and use dashes ("-") instead of spaces.

- For content which is identical for different models or operating  systems, use `include` to avoid duplication of files. Locate those  instructions not in but next to the operating system folders.

- Add a subfolder with the numbered image files for each guide.

- Avoid plain URLs in text but use hyperlinks instead.

- Relative paths (also included and double included) are always evaluated from the path of the final including page. Images within pages that are included elsewhere must therefore always be specified with an absolute path starting with `/` which stands for the root directory of the document.

- add the subheading below the first heading with `.. contents:: :local:`

- add the ToC for local headings in `/$product/$platform/*.rst` with `.. include:: ./product_platform_heading.rst` 

- After each commit, the CI pushes translations automatically. Therefore always do `git pull` before `git commit ...`

- More information about RST:

  https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html

  https://docutils.sourceforge.io/docs/ref/rst/directives.html

## Commits (preventing merge conflicts)
Before pushing your changes to the Github repository, commit often and test the result locally by building a preview with Sphinx (see below). Only push your changes upstream when you are sure you will not touch it for the next hour. If content is pushed twice within an hour, merge conflicts may occur on the Weblate server that need to be solved manually (see below). If it's necessary to edit and push content within an hour, you have to wait until Weblate translated the new content. Then push the commit button in Weblate's web interface (https://translate.nitrokey.com/projects/nitrokey-documentation/#repository) and pull locally on your device. Only then you can push upstream again and avoid merge conflict.
 
 
## Local Preview

Setup Sphinx and components:

```
apt install python3-sphinx
python3 -m pip install divio-docs-theme install sphinx-rtd-theme sphinx-intl sphinxprettysearchresults pygments-style-cheerfully-dark
mkdir -p ~/temp/sphinx_preview
```

For each preview:

```
sphinx-build -a -D language='en' -b html . ~/temp/sphinx_preview
```

Errors about non-existing files in includes can be ignored. Syntax errors and RST files not contained in the TOC are listed.

Open ~/temp/sphinx_preview/index.html In the browser.

## Solving Merge Conflicts

Do not push twice an hour to the repo to avoid merge conflicts. Test locally as described above.

- clone the repo from github
- get the weblate api key from https://translate.nitrokey.com/accounts/profile/#api
- add the remote repo 

```
git remote add weblate https://$USERNAME:$APIKEY@translate.nitrokey.com/git/nitrokey-documentation/index/
```
- Now solve the conflict manually and push upstream e.g.:

```
git remote update weblate
git merge weblate/master
 Auto-merging locales/de/LC_MESSAGES/x230.po
 CONFLICT (content): Merge conflict in locales/de/LC_MESSAGES/x230.po
 Automatic merge failed; fix conflicts and then commit the result.
vim locales/de/LC_MESSAGES/x230.po
  # search for every ">>>>>>>"
  # delete the lines as necessary 
  # save
git status
git add --all
git commit -m "resolve merge conflict"
git push
```
