# MovieWriter

Inherits: Object

Abstract class for non-real-time video recording encoders.

## Description

Godot can record videos with non-real-time simulation. Like the `--fixed-fps`
command line argument, this forces the reported `delta` in Node._process()
functions to be identical across frames, regardless of how long it actually
took to render the frame. This can be used to record high-quality videos with
perfect frame pacing regardless of your hardware's capabilities.

Godot has 2 built-in MovieWriters:

  * AVI container with MJPEG for video and uncompressed audio (`.avi` file extension). Lossy compression, medium file sizes, fast encoding. The lossy compression quality can be adjusted by changing ProjectSettings.editor/movie_writer/mjpeg_quality. The resulting file can be viewed in most video players, but it must be converted to another format for viewing on the web or by Godot with VideoStreamPlayer. MJPEG does not support transparency. AVI output is currently limited to a file of 4 GB in size at most.

  * PNG image sequence for video and WAV for audio (`.png` file extension). Lossless compression, large file sizes, slow encoding. Designed to be encoded to a video file with another tool such as FFmpeg after recording. Transparency is currently not supported, even if the root viewport is set to be transparent.

If you need to encode to a different format or pipe a stream through third-
party software, you can extend the MovieWriter class to create your own movie
writers. This should typically be done using GDExtension for performance
reasons.

Editor usage: A default movie file path can be specified in
ProjectSettings.editor/movie_writer/movie_file. Alternatively, for running
single scenes, a `movie_file` metadata can be added to the root node,
specifying the path to a movie file that will be used when recording that
scene. Once a path is set, click the video reel icon in the top-right corner
of the editor to enable Movie Maker mode, then run any scene as usual. The
engine will start recording as soon as the splash screen is finished, and it
will only stop recording when the engine quits. Click the video reel icon
again to disable Movie Maker mode. Note that toggling Movie Maker mode does
not affect project instances that are already running.

Note: MovieWriter is available for use in both the editor and exported
projects, but it is not designed for use by end users to record videos while
playing. Players wishing to record gameplay videos should install tools such
as OBS Studio or SimpleScreenRecorder instead.

## Methods

int | _get_audio_mix_rate() virtual const  
---|---  
SpeakerMode | _get_audio_speaker_mode() virtual const  
bool | _handles_file(path: String) virtual const  
Error | _write_begin(movie_size: Vector2i, fps: int, base_path: String) virtual  
void | _write_end() virtual  
Error | _write_frame(frame_image: Image, audio_frame_block: `const void*`) virtual  
void | add_writer(writer: MovieWriter) static  
  
## Method Descriptions

int _get_audio_mix_rate() virtual const

Called when the audio sample rate used for recording the audio is requested by
the engine. The value returned must be specified in Hz. Defaults to 48000 Hz
if _get_audio_mix_rate() is not overridden.

SpeakerMode _get_audio_speaker_mode() virtual const

Called when the audio speaker mode used for recording the audio is requested
by the engine. This can affect the number of output channels in the resulting
audio file/stream. Defaults to AudioServer.SPEAKER_MODE_STEREO if
_get_audio_speaker_mode() is not overridden.

bool _handles_file(path: String) virtual const

Called when the engine determines whether this MovieWriter is able to handle
the file at `path`. Must return `true` if this MovieWriter is able to handle
the given file path, `false` otherwise. Typically, _handles_file() is
overridden as follows to allow the user to record a file at any path with a
given file extension:

    
    
    func _handles_file(path):
        # Allows specifying an output file with a `.mkv` file extension (case-insensitive),
        # either in the Project Settings or with the `--write-movie <path>` command line argument.
        return path.get_extension().to_lower() == "mkv"
    

Error _write_begin(movie_size: Vector2i, fps: int, base_path: String) virtual

Called once before the engine starts writing video and audio data.
`movie_size` is the width and height of the video to save. `fps` is the number
of frames per second specified in the project settings or using the `--fixed-
fps <fps>` command line argument.

void _write_end() virtual

Called when the engine finishes writing. This occurs when the engine quits by
pressing the window manager's close button, or when SceneTree.quit() is
called.

Note: Pressing ``Ctrl` + `C`` on the terminal running the editor/project does
not result in _write_end() being called.

Error _write_frame(frame_image: Image, audio_frame_block: `const void*`)
virtual

Called at the end of every rendered frame. The `frame_image` and
`audio_frame_block` function arguments should be written to.

void add_writer(writer: MovieWriter) static

Adds a writer to be usable by the engine. The supported file extensions can be
set by overriding _handles_file().

Note: add_writer() must be called early enough in the engine initialization to
work, as movie writing is designed to start at the same time as the rest of
the engine.

## User-contributed notes

Please read the User-contributed notes policy before submitting a comment.

* * *

Built with Sphinx using a theme provided by Read the Docs.

  *[virtual]: This method should typically be overridden by the user to have any effect.
  *[const]: This method has no side effects. It doesn't modify any of the instance's member variables.
  *[void]: No return value.
  *[static]: This method doesn't need an instance to be called, so it can be called directly using the class name.

