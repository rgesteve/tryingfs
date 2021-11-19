# Trying out F#

Along with a codespaces-based notebook/classic source development environment.

## Using the notebook (need to revise)

This repo is configured with codespaces to use `dotnet-interactive` to drive a jupyter notebook.

Make sure that the right kernels are installed with:
```
$ jupyter kernelspec list
Available kernels:
  .net-csharp        /home/vscode/.local/share/jupyter/kernels/.net-csharp
  .net-fsharp        /home/vscode/.local/share/jupyter/kernels/.net-fsharp
  .net-powershell    /home/vscode/.local/share/jupyter/kernels/.net-powershell
  python3            /usr/local/share/jupyter/kernels/python3
```

If you get 
```
dotnet interactive jupyter install --http-port-range 1100-1200
```

If you see the dotnet kernels installed, you can do:
```bash
$ jupyter notebook --ip=0.0.0.0  --allow-root  --notebook-dir=.
```

If you get an error creating the dotnet-kernel-backed notebook, copy the provided "jupyter_notebook_config.py" to `~/.jupyter`.

## Included notebooks

* FSharpFirst.ipynb : Trying out various curious syntactic forms
