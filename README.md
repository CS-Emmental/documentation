# CS-Emmental Documentation

## Build
To edit this documentation, see the [Sphinx documentation](https://www.sphinx-doc.org/en/master/).

After cloning the repository, you need to create a Python virtual environment and install dependencies:
```sh
python3 -m venv .
source bin/activate
pip install -r requirements.txt
```

Then you can build the documentation:
```sh
make html
```

The resulting files will be in *build/html*.