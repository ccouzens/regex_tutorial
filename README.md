# Regular Expression Tutorial

To run this tutorial:

```bash
docker container run -p 8888:8888 --rm -v "$PWD":/home/jovyan/work:ro --read-only --tmpfs /home/jovyan/.local --tmpfs /tmp jupyter/minimal-notebook
```

This will log out a line like:

```
http://(123330d7df62 or 127.0.0.1):8888/?token=f0eefafeff8a816ea9f0dd865b2ab03ad89918f567991dfc
```

Navigate to the corresponding address in your browser.
eg http://127.0.0.1:8888/?token=f0eefafeff8a816ea9f0dd865b2ab03ad89918f567991dfc

Then navigate to http://127.0.0.1:8888/notebooks/work/Regular%20expression%20tutorial.ipynb
