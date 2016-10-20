# Web Audio API
## General
Provides a system for controlling audio on the web, allowing developers to work with source audio, add effects and conduct analysis.
The API allows audio operations within a *audio context* and has been designed to allow modular routing. Audio nodes are linked togethor to form an audio
routing graph.

Audio nodes are linked into chains and simple webs by their inputs and outputs, the typically start with a source. Sources are arrays of samples from either
computations or recordings. Destination inputs are used to output sound to speakers or headphones.

Typical workflow with web audio woul be:
1. Create an audio context
2. Inside the context, create sources such as <audio>, oscillators or streams
3. Create effect nodes, such as reverb, biquad filter, panner, compressor
4. Choose destination
5. Connect the sources to the effects and the effects to the destination

* The api allows us to control spatialization of the audio, given control over the panning
* processing happens in a seperate thread from the browser ui processing
* calls are made to the audio context which creates a processing graph

## Usage
* Create an audio context using `new AudioContext()`
* Create an audio source ie `const osc = audioContext.createOscillator();`

## Interfaces
* `AudioContext`: Represents an audio processing graph built from audio modules (AudioNodes) linked togethor
  - Controls the create of the nodes it contains and the execution of the audio processing or decoding
  - You need to create an audiocontext before doing anything else
* `AudioNode`: represents an audio processing module, such as an audio source, a destination, a filter etc
  - `.connect(target)`: connects the output of the current node, to the input of the target node
* `AudioParam`: Represents an audio related parameter, it can be set to a specific value or a change in value
  - it can be scheduled to occur at a specific time or to follow a pattern
* `ended(event)`: fires when playback reaches the end of the media and has stopped

### Sources
* `OscillatorNode`: represents a sine wave
  - `.type`: attribute to specify the type of oscillator, can be *sine*, *square*, *sawtooth*, *triangle*, *custom*
  - `.frequency.value`: the frequency of the wave in hz

* `AudioBuffer`: a short audio asset living in memory
  - can be created from an audio file using `AudioContext.decodeAudioData()` or with raw data using `AudioContext.createBuffer()`
  - once it has been created it can be put into an `AudioBufferSourceNode`
* `MediaElementAudioSourceNode`: an audio source consisting of a <audio> or <video> element
  - it is an AudioNode that acts as an AudioSource
* `MediaStreamAudioSourceNode`: an audio source consisting of WebRTC MediaStream such as a webcam or microphone

### Audio Effects
* `BiquadFilterNode`: a simple low order filter
 - can be used to represent different kinds of filters, tone control devices or equalizers
 - has exactly 1 input and 1 output
* `ConvolverNode`: performs a linear convolution on a given AudioBuffer
  - can be used to create a reverb
* `DelayNode`: represents a delay-line and causes a delay between the arrival of an input data and its propagation to the output
* `DynamicsCompressorNode`: provides a compression effect
* `GainNode`: represents a change in volume
  - causes a given gain to be applied to the input data before it propagates to the output
* `StereoPannerNode`: simple stereo panner to move audio left or right
* `WaveShaperNode`: a non-linear distorter that uses a curve to apply a waveshaping distortion to the signal
* `PeriodicWave`: a waveform that can be used to shape the output of an OscillatorNode

### Destinations
* `AudioDestinationNode`: the end destination of an audio source in a given context
* `MediaStreamAudioDestinationNode`: represents an audio destination consisting of a WebRTC MediaStream with a single AudioMediaStreamTrack

### Analysis
* `AnalyserNode`: provides real-time frequency and time domain analysis

### Splitting and merging
* `ChannelSpliterNode`: separates the channels of an audio source into a set of mono outputs
* `ChannelMergerNode`: combines mono inputs into a single output
  - each input will be used to fill a channel of the output

### Spatialization
* `AudioListener`: represent the position and orientation of the unique person listening to the audio scene
* `PannerNode`: the behaviour of a signal in space
  - it is an AudioNode audio-processing module
  - uses a right hand cartesian coordinate for its position
  - movement is represented with a velocity vector
  - directionality specified with a directionality cone

### Processing
* `ScriptProcessorNode`: allows the generation, procsssing or analysis of audio
  - it is linked to two buffers, one for input and one for output
  - an event implementing `AudioProcessingEvent` is sent to the object each time the input buffer contains new data, this event terminates when it has filled the output buffer
* `audioprocess(event)`: fired when an input buffer is ready to be processed
* `OfflineAudioContext`: represents an audio processing graph built from linked AudioNodes
  - in contrast to a AudioContext, it doesnt render the audio but just generates it as fast as it can into a buffer
* `complete(event)`: fired when the rendering of an OfflineAudioContext is termintated
* `OfflineAudioCompletionEvent`: occurs when the processing of an OfflineAudioContext is termintated, the complete event implements this interface

## Links
* [MDN reference](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
* [HTML5 Rocks tutorial](https://www.html5rocks.com/en/tutorials/webaudio/intro/)
* [Web audio UIs](https://www.youtube.com/watch?v=tSThM9Aw8ps)