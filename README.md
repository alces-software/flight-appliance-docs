#Alces Flight Appliance Documentation
[![Documentation Status](https://readthedocs.org/projects/alces-flight-appliance-docs/badge/?version=latest)](http://alces-flight-appliance-docs.readthedocs.org/en/aws/?badge=latest)


Documentation to support deployment and usage of Alces Flight Appliances

* [Read the documentation](http://alces-flight-appliance-docs.readthedocs.org/en/latest/)

## Writing documentation

Assuming you have Python installed, install the dependencies by running the
following:

```bash
pip install sphinx sphinx-autobuild sphinx_rtd_theme
```

You can then make your changes to the pages you want to edit; see
[here](http://www.sphinx-doc.org/en/stable/rest.html) for a syntax reference.

To preview your changes:

```bash
cd docs
make html
xdg-open build/html/$file
```

where `$file` is the path to the file to view within the `docs/source`
directory, with `.html` rather than `.rst` extension.
