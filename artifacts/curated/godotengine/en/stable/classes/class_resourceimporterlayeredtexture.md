# ResourceImporterLayeredTexture

Inherits: ResourceImporter < RefCounted < Object

Imports a 3-dimensional texture (Texture3D), a Texture2DArray, a Cubemap or a
CubemapArray.

## Description

This imports a 3-dimensional texture, which can then be used in custom
shaders, as a FogMaterial density map or as a
GPUParticlesAttractorVectorField3D. See also ResourceImporterTexture and
ResourceImporterTextureAtlas.

## Tutorials

  * Importing images

## Properties

int | compress/channel_pack | `0`  
---|---|---  
int | compress/hdr_compression | `1`  
bool | compress/high_quality | `false`  
float | compress/lossy_quality | `0.7`  
int | compress/mode | `1`  
bool | mipmaps/generate | `true`  
int | mipmaps/limit | `-1`  
int | slices/arrangement | `1`  
  
## Property Descriptions

int compress/channel_pack = `0`

Controls how color channels should be used in the imported texture.

sRGB Friendly:, prevents the RG color format from being used, as it does not
support sRGB color.

Optimized:, allows the RG color format to be used if the texture does not use
the blue channel. This reduces memory usage if the texture's blue channel can
be discarded (all pixels must have a blue value of `0`).

Normal Map (RG Channels): This forces all layers from the texture to be
imported with the RG color format, with only the red and green channels
preserved. RGTC (Red-Green Texture Compression) compression is able to
preserve its detail much better, while using the same amount of memory as a
standard RGBA VRAM-compressed texture. This only has an effect on textures
with the VRAM Compressed or Basis Universal compression modes. This mode is
only available in layered textures (Cubemap, CubemapArray, Texture2DArray and
Texture3D).

int compress/hdr_compression = `1`

Controls how VRAM compression should be performed for HDR images.

Disabled: Never use VRAM compression for HDR textures, regardless of whether
they're opaque or transparent. Instead, the texture is converted to RGBE9995
(9-bits per channel + 5-bit exponent = 32 bits per pixel) to reduce memory
usage compared to a half-float or single-precision float image format.

Opaque Only: Only uses VRAM compression for opaque HDR textures. This is due
to a limitation of HDR formats, as there is no VRAM-compressed HDR format that
supports transparency at the same time.

Always: Force VRAM compression even for HDR textures with an alpha channel. To
perform this, the alpha channel is discarded on import.

Note: Only effective on Radiance HDR (`.hdr`) and OpenEXR (`.exr`) images.

bool compress/high_quality = `false`

If `true`, uses BPTC compression on desktop platforms and ASTC compression on
mobile platforms. When using BPTC, BC7 is used for SDR textures and BC6H is
used for HDR textures.

If `false`, uses the faster but lower-quality S3TC compression on desktop
platforms and ETC2 on mobile/web platforms. When using S3TC, DXT1 (BC1) is
used for opaque textures and DXT5 (BC3) is used for transparent or normal map
(RGTC) textures.

BPTC and ASTC support VRAM compression for HDR textures, but S3TC and ETC2 do
not (see compress/hdr_compression).

float compress/lossy_quality = `0.7`

The quality to use when using the Lossy compression mode. Higher values result
in better quality, at the cost of larger file sizes. Lossy quality does not
affect memory usage of the imported texture, only its file size on disk.

int compress/mode = `1`

The compression mode to use. Each compression mode provides a different
tradeoff:

Lossless: Original quality, high memory usage, high size on disk, fast import.

Lossy: Reduced quality, high memory usage, low size on disk, fast import.

VRAM Compressed: Reduced quality, low memory usage, low size on disk, slowest
import. Only use for textures in 3D scenes, not for 2D elements.

VRAM Uncompressed: Original quality, high memory usage, highest size on disk,
fastest import.

Basis Universal: Reduced quality, low memory usage, lowest size on disk, slow
import. Only use for textures in 3D scenes, not for 2D elements.

See Compress mode in the manual for more details.

bool mipmaps/generate = `true`

If `true`, smaller versions of the texture are generated on import. For
example, a 6464 texture will generate 6 mipmaps (3232, 1616, 88, 44, 22, 11).
This has several benefits:

  * Textures will not become grainy in the distance (in 3D), or if scaled down due to Camera2D zoom or CanvasItem scale (in 2D).

  * Performance will improve if the texture is displayed in the distance, since sampling smaller versions of the original texture is faster and requires less memory bandwidth.

The downside of mipmaps is that they increase memory usage by roughly 33% (for
Texture2DArray, Cubemap and CubemapArray) or 14% (for Texture3D).

It's recommended to enable mipmaps in 3D. However, in 2D, this should only be
enabled if your project visibly benefits from having mipmaps enabled. If the
camera never zooms out significantly, there won't be a benefit to enabling
mipmaps but memory usage will increase.

int mipmaps/limit = `-1`

Unimplemented. This currently has no effect when changed.

int slices/arrangement = `1`

Controls how the cubemap's texture is internally laid out. When using high-
resolution cubemaps, 23 and 32 are less prone to exceeding hardware texture
size limits compared to 16 and 61.

## User-contributed notes

Please read the User-contributed notes policy before submitting a comment.

* * *

Built with Sphinx using a theme provided by Read the Docs.

