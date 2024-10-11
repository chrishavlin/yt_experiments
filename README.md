# yt_experiments

The `yt_experiments` package contains experimental functionality and enhancements to use with
[yt](https://github.com/yt-project/yt). Eventually some of the functionality here may make its
way over to yt, but until it does you can test out the latest new features that were too big or
too experimental or too peripheral to go into yt directly right away. You should, however, consider
any functionality in `yt_experiments` to be in beta and subsequent releases of `yt_experiments` may
introduce breaking API changes (but will be noted in release notes!).

## Features

Currently in `yt_experiments`, you will find the following:

* The `OctTree` converter utility for converting a generic octree in list-form to a format that you can pass over to `yt.load_octree`([Example Usage](#ytexperimentsoctreeconverterocttree)).

## Installation

### from pypi

Install the latest release with `pip` into your active environment with

```commandline
python -m pip install yt_experiments
```

### from source

Clone (or fork and clone) this repository and `cd` into the resulting directory:

```commandline
python -m pip install .
```

## Example Usage

### `yt_experiments.octree.converter.OctTree`

To covert a list of cells (defined by cell-centered values):

```python
import numpy as np
import yt
from yt_experiments.octree.converter import OctTree

xyz = np.array(
    [
        [0.375, 0.125, 0.125],
        [0.125, 0.125, 0.125],
        [0.375, 0.375, 0.375],
        [0.75, 0.75, 0.75],
    ]
)
levels = np.array([2, 2, 2, 1], dtype=np.int32)
data = {("gas", "density"): np.random.rand(4)}
oct = OctTree.from_list(xyz, levels, check=True)
ref_mask, leaf_order = oct.get_refmask()
print(ref_mask, leaf_order)
# (array([1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], dtype=uint8),
#  array([ 1, -1, -1, -1,  0, -1, -1,  2, -1, -1, -1, -1, -1, -1,  3], dtype=int32))
for k, v in data.items():
    # Make it 2D so that yt doesn't think those are particles
    data[k] = np.where(leaf_order >= 0, v[leaf_order], np.nan)[:, None]
ds = yt.load_octree(ref_mask, data)
```

For more details on expected parameters, check the docstring for `OctTree.from_list`.

## contributing

Contributions are welcome!

Check out the [contributing docs](contributing.md) for more details.
