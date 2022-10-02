# SoftVC VITS Singing Voice Conversion
## 模型简介
歌声音色转换模型，通过SoftVC内容编码器提取源音频语音特征，通过icassp2022_vocal_transcription项目提取音频midi note，将两者结合输入VITS替换原本的文本输入达到歌声转换的效果。
> 该midi方案目前暂时搁置转入dev分支，目前模型修改回使用 [coarse F0输入](https://github.com/PlayVoice/VI-SVC/blob/main/svc/prepare/preprocess_wave.py)
## 实现细节
+ midi note（0-127 LongTensor）通过pitch_embedding后与soft-units相加替代vits原本的文本输入
  + 使用midi而非f0似乎会导致模型音高不准 目前修改回使用F0
+ 采用了VISinger中的PitchPredictor，引入了PitchLoss修正音高
  + 似乎效果不是很明显，或许加的方式不太对
  + 
##改版说明
#基于原版进行了一定更改，使得程序可以直接从系统默认麦克风录音，并在处理后由系统默认扬声器输出
致命缺陷：模型对于长度过短的音频处理能力有限，录音不得短于2秒，这使得输出音频不可避免存在2-3秒延迟
