pyodide-playground
==================
- [Pyodide — Version 0.24.1](https://pyodide.org/en/stable/index.html)

### Notes
- matplotlib needs this workaround to run on web workers (see [pyodide/matplotlib-pyodide](https://github.com/pyodide/matplotlib-pyodide))
  - ```python
    from matplotlib import pyplot as plt
    import io
    import base64
    import js
    
    
    class Dud:
    
        def __init__(self, *args, **kwargs) -> None:
            return
    
        def __getattr__(self, __name: str):
            return Dud
    
    
    js.document = Dud()
    # Create a plot
    x1, y1 = [-1, 12], [1, 4]
    plt.plot(x1, y1)
    
    # Print base64 string to stdout
    bytes_io = io.BytesIO()
    
    plt.savefig(bytes_io, format='jpg')
    
    bytes_io.seek(0)
    
    base64_encoded_spectrogram = base64.b64encode(bytes_io.read())
    
    # TODO: print this data uri to canvas
    print("data:image/jpeg;base64" + base64_encoded_spectrogram.decode('utf-8'))
    ```
  - [Matplotlib backend in a web worker · Issue #6 · pyodide/matplotlib-pyodide](https://github.com/pyodide/matplotlib-pyodide/issues/6)
  - [Pyolite - Matplotlib: Setup inline backend by default by martinRenou · Pull Request #911 · jupyterlite/jupyterlite](https://github.com/jupyterlite/jupyterlite/pull/911)
  - [HTML5 <canvas> based renderer for Matplotlib in Pyodide | Pyodide blog](https://blog.pyodide.org/posts/canvas-renderer-matplotlib-in-pyodide/)
### Snippets
```python
from js import document
canvas_element = document.getElementById("canvas")
ctx = canvas_element.getContext("2d")


```
