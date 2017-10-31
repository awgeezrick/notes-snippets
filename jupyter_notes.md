# Jupyter Notebook
Notes and Other Useful Things
--------------------
# Summary
There are my personal notes, code snippets, etc. for using Jupyter notebook. These are basically a collections of Jupyter related tricks, tips, and functions I have gathered from the around the internet, found useful, and don't want to forget.

# Contents
- [Display settings](#display)
- [R + Jupyter](#r)
- [Execute Shell Commands](#shell)
- [LaTeX](#latex)
- [Magics](#magics)

<a id="display"></a>
## Display Settings

#### Display matplotlib plots inline in notebook
`%matplotlib inline`
#### Display the value of multiple statements at once
```py
# will display both head and tail if you
#    call both functions in a single cell
# without this, only the first statement would
#    be visible when run
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
```
__To set this globally, add the following to the
ipython config file...__
`~/.ipython/profile_default/ipython_config.py`
```py
c = get_config()
# Run all nodes interactively
c.InteractiveShell.ast_node_interactivity = "all"
```
#### Disable warning messages in notebook
```py
import warnings
warnings.simplefilter('ignore')
```
<a id="shell"></a>
## Execute Shell Commands
To execute a shell command from within Jupyter notebook, use `!`
```py
# To check available csv's in working directory
!ls *.csv

# To install packages
!pip install numpy
```
<a id="latex"></a>
## LaTeX
LaTex formulas will be rendered in Markdown cells using MathJax
```latex
$$ P(A \mid B) = \frac{P(B \mid A) \, P(A)}{P(B)} $$
```
