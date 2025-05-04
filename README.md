# pyodide-build

Build Pyodide via GH Actions

## Building a Custom JupyterLite Site

- Describe motivation, background, architecture
  - What are the pieces?
  - Why use static sites?
  - Limitations
  - Benefits
- Pyodide takes a long time to build
- Having the JupyterLite site in a separate repo makes it quick to update data and notebooks
- Multiple JupyterLite sites can reuse the same Pyodide distribution

### Get Ready

- Gather your data and notebooks
- Build a list of all the packages imported in your notebooks or local Python modules
- Look at packages [already in Pyodide](https://pyodide.org/en/stable/usage/packages-in-pyodide.html) and see if you need to build a custom Pyodide distribution
- Look at the list of package in the [Python Standard Library](https://docs.python.org/3/py-modindex.html)
- If your package isn't in there, look through the dependencies
  - provide the notebook to help folks find the list

### Build a Custom Pyodide Distribution

- Fork the template repo
  - need to create this based on existing one
  - add a requirements.txt file with pyodide-build and prettier
  - maybe put just these docs in there
- Configure github pages build with actions
- update repo landing page settings to show link
- Kick off build, check the results
- Clone the repo
- Venv `python -m venv venv`, `source venv/bin/activate`
- install dependencies `pip install -r requirements.txt`
- For each dependency create a [skeleton yaml file](https://pyodide.org/en/stable/development/new-packages.html#creating-the-meta-yaml-file)
```
pyodide skeleton pypi <package name>
```
- update download? wheel or tarball?
- definitely add the dependencies
- git add, commit, push
- watch the build process
- can also build locally
  - more advanced process
- test by going to your repo and checking the build and deploy action status
- once it's built, go to console and run a test
  - good to test even an import to make sure dependencies were listed correctly
- need to define a way to keep the list of packages to build up-to-date
  - git commit hook?
  - '*'?


### Build a Custom JupyterLite Site

- Clone the template repo
  - Should I create a smaller template repo?
- Set the GitHub actions build for GitHub Pages
- Edit the README if you like
- Remove any notebooks and data from `content/` that you're not going to use
- Add your notebooks and data, plus any other files and folders you like to 'content/`
  - These files get copied to the root of the virtual file system
- If you built a custom Pyodide distribution, update the `jupyter-lite.json` configuration file in your JupyterLite repo to add the `pyodideUrl` attribute.
```
{
  "jupyter-lite-schema-version": 0,
  "jupyter-config-data": {
    "litePluginSettings": {
      "@jupyterlite/pyodide-kernel-extension:kernel": {
        "pyodideUrl": "https://{username}.github.io/pyodide-build/pyodide.js"
      }
    }
  }
}
```

