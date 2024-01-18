The following commands should be executed in the terminal. You need to run 'build' if you have made any changes to the Dockerfile or pipeline.py file

```bash
docker build -t test:pandas .
docker run -it test:pandas
```