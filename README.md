# NTIRE 2022 Challenge on Super-Resolution and Quality Enhancement of Compressed Video

The homepage for the NTIRE 2022 Challenge on Super-Resolution and Quality Enhancement of Compressed Video.  [[Report]](https://openaccess.thecvf.com/content/CVPR2022W/NTIRE/papers/Yang_NTIRE_2022_Challenge_on_Super-Resolution_and_Quality_Enhancement_of_Compressed_CVPRW_2022_paper.pdf)

This challenged is organized by [Ren Yang](https://renyang-home.github.io/) (ETH Zurich) and [Prof. Dr. Radu Timofte](https://people.ee.ethz.ch/~timofter/) (ETH Zurich, Julius Maximilian University of Wurzburg) as a part of the [NTIRE 2022 Workshop](https://data.vision.ee.ethz.ch/cvl/ntire22/) in conjunction with [CVPR 2022](https://cvpr2022.thecvf.com/).

If the LDV 2.0 dataset and the benchmark are useful for your research, please cite:
```
@inproceedings{yang2022ntire,
  title={{NTIRE 2022} Challenge on Super-Resolution and Quality Enhancement of Compressed Video: Dataset, Methods and Results},
  author={Ren Yang and Radu Timofte and others}, 
  booktitle={IEEE/CVF Conference on Computer Vision and Pattern Recognition Workshops}, 
  year={2022}
}
```

If you have questions, find dead links or bugs, please feel free to contact:

Ren Yang @ ETH Zurich, Switzerland   

Email: ren.yang@vision.ee.ethz.ch

## LDV 2.0 Dataset

We use the LDV 2.0 dataset in this Challenge. Please find the dataset and the splits of training, validation and test sets at https://github.com/RenYang-home/LDV_dataset#ldv-20-ldv-10--95-videos

### Challenge

In Track 1, videos are compressed in the YUV domain by the Low-delay P mode HM 16.20 at QP 37. In Track 2, videos are first x2 downsampled and then compressed in the YUV domain by the Low-delay P mode HM 16.20 at QP 37. In Track 3, videos are first x4 downsampled and then compressed in the YUV domain by the Low-delay P mode HM 16.20 at QP 37.

The raw YUV videos in the LDV 2.0 dataset are losslessly compressed to mkv via ffmpeg x265 to reduce the file sizes.

1. To generate the compressed data, first convert the raw mkv files to YUV domain. The ffmpeg x265 decoder is recommended, e.g., “ffmpeg -i 001.mkv -pix_fmt yuv420p 001.yuv”

2. Downsample the videos by x2 bicubic (Track 2) or x4 bicubic (Track 3) using the commands below. We define ```w``` and ```h``` as the width and height after downsampling. In Track 1, ```w = width, h = height```. In Track 2, ```w = width/2, h = height/2```. In Track 3, ```w = width/4, h = height/4```

```
ffmpeg -pix_fmt yuv420p -s (width)x(height) -i xxx.yuv -vf scale=(w)x(h):flags=bicubic xxx.yuv
```

3. Download HM 16.20 at https://hevc.hhi.fraunhofer.de/svn/svn_HEVCSoftware/tags/HM-16.20 or https://data.vision.ee.ethz.ch/reyang/HM16.20.zip

4. Compress yuv files by the command:

```
path_to_HM16.20/bin/TAppEncoderStatic 
-c path_to_HM16.20/cfg/encoder_lowdelay_P_main.cfg 
-c path_to_HM16.20/cfg/per-sequence/BasketballDrill.cfg 
-i xxx.yuv -q 37 -wdt (w) -hgt (h)
-f (frame_num) -fr (frame_rate) -b xxx.mkv
```

Note that “BasketballDrill.cfg” is a randomly selected file, and most of its information are replaced by the following configurations. Width, height, frame number and frame rate values are available in the info excel files (refer to [[LDV 2.0 Dataset]](https://github.com/RenYang-home/LDV_dataset#ldv-20-ldv-10--95-videos)).

5. The quality is evaluated in the RGB domain by PSNR. Please convert raw, compressed (and enhanced) videos to RGB domain for evaluation, e.g.,
```
ffmpeg -i path_to_raw/001.mkv ./raw_001/%3d.png
ffmpeg -i path_to_compressed/001.mkv ./001/%3d.png
```

## NTIRE 2022 Benchmark

The methods and results of the NTIRE 2022 benchmark are discribed in the report:

> Ren Yang, Radu Timofte, et al., "NTIRE 2022 Challenge on Super-Resolution and Quality Enhancement of Compressed Video: Dataset, Methods and Results", in IEEE/CVF Conference on Computer Vision and Pattern Recognition Workshops, 2022. [[Paper]](http://arxiv.org/abs/2204.09314)

To make the benchmark more convincing and solid, we will update the open-source codes of the proposed methods in the following.

- **TaoMC2 Team** (Contact: Meisong Zheng, zhengmeisong.zms@alibaba-inc.com)

  Winner of Track 1 and Track 2
  
  - Paper: https://arxiv.org/abs/2204.09924

  - Homepage: https://github.com/ryanxingql/winner-ntire22-vqe

- **OCL-VCE Team** (Contact: Thong Bach, thongbtqm@gmail.com)

  - Code: https://drive.google.com/file/d/15wwKhBJgQRpr0oCOuxPc8jVnccN0cOyw/view?usp=sharing

- **BasicVSR++** (Contact: Kelvin C.K. Chan, chan0899@e.ntu.edu.sg)

  Winner method in NTIRE 2021
  
  - Paper: https://arxiv.org/abs/2104.13371

  - Code: https://github.com/open-mmlab/mmediting/tree/master/configs/restorers/basicvsr_plusplus
