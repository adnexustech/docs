# ADNEXUS

ADNEXUS is an enterprise strength bidding platform for RTB based digital advertising.  ADNEXUS has the following functionality:

1. Campaign management
2. Data management (DMP) capability
3. Bidding using many different strategies
4. Analytics on historical media and pricing information

The [ADNEXUS](https://github.com/ADNEXUS) organization on Github contains all of the repos to run a fully scalable system.

The ADNEXUS/[adnexus](https://github.com/ADNEXUS/adnexus) repo is where all ADNEXUS user documentation, developer documentation, and API explorer documentation live.

## Viewing ADNEXUS Documentation

To view the documentation, navigate to:

https://adnexus.readthedocs.io/en/latest/

## Building ADNEXUS Documentation

To build a local copy of the ADNEXUS documentation, follow these steps:

### Clone the adnexus repo

Make a local directory and clone this repo:

```
mkdir adnexus
cd adnexus
git clone git@github.com:switzer/adnexus.git
cd adnexus
```
### Build the user documentation

Build the user documentation, which is built on [Sphinx](http://www.sphinx-doc.org/en/master/) and [ReadTheDocs](https://readthedocs.org/)

```
cd docs
make html
```

This will build HTML doc in the `adnexus/docs/_build/html` directory.  Open the `index.html` file to view the documentation on your local system.





