# ome-types: OME dataclasses for python

autogenerated dataclasses for a pythonic interface into the OME data model:
http://www.openmicroscopy.org/Schemas/OME/2016-06

It converts the ome.xsd schema into a set of python dataclasses and types.

As an example, the
[`OME/Image`](https://www.openmicroscopy.org/Schemas/Documentation/Generated/OME-2016-06/ome.html)
model will be rendered as the following dataclass in `ome_types/model/image.py`

```python
from dataclasses import field
from datetime import datetime
from typing import List, Optional

from pydantic.dataclasses import dataclass

from .annotation_ref import AnnotationRef
from .experiment_ref import ExperimentRef
from .experimenter_group_ref import ExperimenterGroupRef
from .experimenter_ref import ExperimenterRef
from .imaging_environment import ImagingEnvironment
from .instrument_ref import InstrumentRef
from .microbeam_manipulation_ref import MicrobeamManipulationRef
from .objective_settings import ObjectiveSettings
from .pixels import Pixels
from .roi_ref import ROIRef
from .simple_types import ImageID
from .stage_label import StageLabel


@dataclass
class Image:
    id: ImageID
    pixels: Pixels
    acquisition_date: Optional[datetime] = None
    annotation_ref: List[AnnotationRef] = field(default_factory=list)
    description: Optional[str] = None
    experiment_ref: Optional[ExperimentRef] = None
    experimenter_group_ref: Optional[ExperimenterGroupRef] = None
    experimenter_ref: Optional[ExperimenterRef] = None
    imaging_environment: Optional[ImagingEnvironment] = None
    instrument_ref: Optional[InstrumentRef] = None
    microbeam_manipulation_ref: List[MicrobeamManipulationRef] = field(default_factory=list)
    name: Optional[str] = None
    objective_settings: Optional[ObjectiveSettings] = None
    roi_ref: List[ROIRef] = field(default_factory=list)
    stage_label: Optional[StageLabel] = None
```


`ome_autogen.convert_schema(url, target)` is the main function.  It accepts an
`xsd` file path (only test on an ome.xsd), and a target directory, and writes a
human-readable module, that will validate OME XML, provide pythonic method
naming, and provides full typing support for IDEs, etc...

## Install

from pip

```bash
pip install ome-types
```

the autogenerated model is already included, but, to include dependencies
required for re-generating the model, use the `[autogen]` extra

```bash
pip install ome-types[autogen]
```

or, to install from source

```bash
git clone https://github.com/tlambert03/ome-types.git
cd ome-types
pip install -e .
```

## Usage

The model is not checked into source, but it is included when you pip install
the package (and it will be built automatically at `ome_types/model` if it
doesn't exist the first time you import the package.)

```python
from ome_types import OME  # the root class

# or specific objects
from ome_types.model import Image, Pixels, Plate  # etc...
```

There is a convenience function that accepts xml, and outputs a *validated* OME
model (if it fails validation, an exception is raised):

```python
from ome_types import from_xml

metadata = from_xml(xml)
```

where `xml` in that example can be a path to a file, a URI of a
resource, an opened file-like object, an Element instance, an ElementTree
instance, or a literal string containing the XML data.

all attributes and variable names follow the OME data model, but `camelCaseNames`
have been replaced with pythonic `snake_case_names`

## Work in progress!

This is a work in progress and will absolutely need refining.  Feel free to
submit an issue or a PR if you try it out and have requests.