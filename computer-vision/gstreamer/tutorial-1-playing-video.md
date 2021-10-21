# Tutorial 1: Playing Video

**GStreamer** is a framework designed to handle multimedia flows. Media travels from the "source" elements (the producers), down to the "sink" elements (the consumers), passing through a series of intermediate elements performing all kinds of tasks. The set of all the interconnected elements is called a "pipeline"

we are going to play a video. [Link](https://gstreamer.freedesktop.org/documentation/tutorials/basic/hello-world.html?gi-language=python#gst\_parse\_launch)

{% code title="basic-tutorial-1.py" %}
```python
#!/usr/bin/env python3
import sys

import gi

gi.require_version('GLib', '2.0')
gi.require_version('GObject', '2.0')
gi.require_version('Gst', '1.0')

from gi.repository import Gst, GObject, GLib

pipeline = None
bus = None
message = None

# initialize GStreamer
Gst.init(sys.argv[1:])

# build the pipeline
pipeline = Gst.parse_launch(
    "playbin uri=https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm"
)

# start playing
pipeline.set_state(Gst.State.PLAYING)

# wait until EOS or error
bus = pipeline.get_bus()
msg = bus.timed_pop_filtered(
    Gst.CLOCK_TIME_NONE,
    Gst.MessageType.ERROR | Gst.MessageType.EOS
)

# free resources
pipeline.set_state(Gst.State.NULL)
```
{% endcode %}
