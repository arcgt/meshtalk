# meshtalk

This repository contains code to run [MeshTalk](https://openaccess.thecvf.com/content/ICCV2021/papers/Richard_MeshTalk_3D_Face_Animation_From_Speech_Using_Cross-Modality_Disentanglement_ICCV_2021_paper.pdf) for face animation from audio. If you use MeshTalk, please cite
```
@inproceedings{richard2021meshtalk,
    author    = {Richard, Alexander and Zollh\"ofer, Michael and Wen, Yandong and de la Torre, Fernando and Sheikh, Yaser},
    title     = {MeshTalk: 3D Face Animation From Speech Using Cross-Modality Disentanglement},
    booktitle = {Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV)},
    month     = {October},
    year      = {2021},
    pages     = {1173-1182}
}
```

## Supplemental Material
[![Watch the video](https://github.com/facebookresearch/meshtalk/blob/main/supplemental_video.png)](https://www.facebook.com/MetaResearch/videos/251508987094387/)

## Running MeshTalk

### Dependencies

```
ffmpeg
numpy
torch         (tested with v1.10.0)
pytorch3d     (tested with v0.4.0)
torchaudio    (tested with v0.10.0)
```

To create a new environment with the required dependencies, run
```
conda install pytorch=1.10.0 torchvision torchaudio cudatoolkit=11.3 -c pytorch -c conda-forge
conda install -c fvcore -c iopath -c conda-forge fvcore iopath
conda install -c pytorch3d pytorch3d
conda install ffmpeg numpy
conda update ffmpeg
```

### Animating a Face Mesh from Audio

Download the [pretrained models](https://github.com/facebookresearch/meshtalk/releases/download/pretrained_models_v1.0/pretrained_models.zip) and unzip them.
Make sure your python path contains the root directory (`export PYTHONPATH=<your_meshtalk_root_directory>`).

Then, run
```
python animate_face.py --model_dir <your_pretrained_model_dir> --audio_file <your_speech_snippet.wav> --output <your_output_file.mp4>
```
See a description of command line arguments via `python animate_face.py --help`. We provide a neutral face template mesh in `assets/face_template.obj`. Note that the rendered results look slightly different than in the paper and supplemental video because we use a differnt (open source) rendering engine in this repository.

### Training your own MeshTalk version

A subset of the data (13 subjects) has been released as our [Multiface](https://github.com/facebookresearch/multiface) dataset. The dataset includes tracked meshes and audio files.

Note that the geometries in multiface have a slightly different topology than in meshtalk. To convert geometries from multiface to meshtalk, run
```
python utils/multiface2meshtalk.py <multiface_mesh.bin> <output.obj>
```
on the `.bin` files containing the vertex positions of the multiface meshes. Note that the input must be the `.bin` files from the `tracked_mesh` directories in multiface, **not** the `.obj` files. The output is a `.obj` file in the same format as `assets/face_template.obj`.

## License

The code and dataset are released under [CC-NC 4.0 International license](https://github.com/facebookresearch/BinauralSpeechSynthesis/blob/main/LICENSE).
