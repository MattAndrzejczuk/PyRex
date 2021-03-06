# PyRex
Listen to your default input source i.e. Microphone and save it to a .wav file.

#### CC BY Matt Andrzejczuk

###  Mac OS X Dependencies
`brew install portaudio`

`pip3 install pyaudio`

###  Ubuntu Dependencies 
`sudo apt-get install python-pyaudio python3-pyaudio`

`pip3 install pyaudio`


### Overview
#####                listen_audio.py

```python
import pyaudio
import wave

CHUNK = 1024
FORMAT = pyaudio.paInt16
CHANNELS = 2
RATE = 44100
RECORD_SECONDS = 8
WAVE_OUTPUT_FILENAME = "output.wav"

p = pyaudio.PyAudio()

stream = p.open(format=FORMAT,
                channels=CHANNELS,
                rate=RATE,
                input=True,
                frames_per_buffer=CHUNK)

print("* recording")

frames = []

for i in range(0, int(RATE / CHUNK * RECORD_SECONDS)):
    data = stream.read(CHUNK)
    frames.append(data)

print("* done recording")

stream.stop_stream()
stream.close()
p.terminate()

wf = wave.open(WAVE_OUTPUT_FILENAME, 'wb')
wf.setnchannels(CHANNELS)
wf.setsampwidth(p.get_sample_size(FORMAT))
wf.setframerate(RATE)
wf.writeframes(b''.join(frames))
wf.close()

```

#####  Convert Output File to ByteString:
```python
def bytesfromfile(f):
    while True:
        raw = array.array('B')
        raw.fromstring(f.read(8192))
        if not raw:
            break
        yield raw
        
        
with open(f_in, 'rb') as fd_in:
    for byte in bytesfromfile(fd_in):
        # do stuff
```

##### run:

`python3 listen_audio.py`
