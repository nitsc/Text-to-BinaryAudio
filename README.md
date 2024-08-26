# 文本转音频信号程序

这是一个将文本转换为音频信号的 Python 程序。该程序将输入的文本转换为二进制信号，并将这些信号映射到音频波形中，最终生成一个可下载的音频文件。该程序在 Google Colab 上运行，以便于快速处理和下载音频文件。

## 功能

- 将输入的文本转换为二进制字符串。
- 将二进制字符串映射为不同频率的音频信号。
- 生成并下载包含文本信号的 WAV 音频文件。

## 使用说明

### 在 Google Colab 中运行

1. 打开 [Google Colab](https://colab.research.google.com/)。
2. 新建一个 Colab 笔记本。
3. 复制并粘贴以下代码到一个新的代码单元格中：

    ```python
    import numpy as np
    from scipy.io.wavfile import write
    
    def text_to_binary(text):
        """将文本转换为二进制字符串"""
        binary_string = ''.join(format(ord(char), '08b') for char in text)
        return binary_string
    
    def binary_to_audio(binary_string, sample_rate=44100, bit_rate=10):
        """将二进制字符串转换为音频信号"""
        duration_per_bit = 1.0 / bit_rate
        samples_per_bit = int(sample_rate * duration_per_bit)
        freq_0 = 440  # 频率为440Hz的音符（代表'0'）
        freq_1 = 880  # 频率为880Hz的音符（代表'1'）
    
        audio_signal = np.array([])
        for bit in binary_string:
            if bit == '0':
                t = np.linspace(0, duration_per_bit, samples_per_bit, endpoint=False)
                wave = np.sin(2 * np.pi * freq_0 * t)
            else:
                t = np.linspace(0, duration_per_bit, samples_per_bit, endpoint=False)
                wave = np.sin(2 * np.pi * freq_1 * t)
            audio_signal = np.concatenate((audio_signal, wave))
    
        # 归一化到[-1, 1]
        audio_signal = audio_signal / np.max(np.abs(audio_signal))
        return audio_signal
    
    def save_audio(filename, audio_signal, sample_rate=44100):
        """保存音频信号为WAV文件"""
        write(filename, sample_rate, audio_signal.astype(np.float32))
    
    # 运行时输入需要转换的文字
    text = input("请输入需要转换的文字：")
    
    binary_string = text_to_binary(text)
    audio_signal = binary_to_audio(binary_string)
    output_path = f'{text}_audio.wav'
    save_audio(output_path, audio_signal)
    
    print(f"Audio file generated as '{output_path}'")
    
    # 下载生成的音频文件
    from google.colab import files
    files.download(output_path)

    ```

4. 运行代码单元格时，系统会提示你输入需要转换的文字。
5. 输入文字后，程序将生成音频文件并提供下载链接。

### 依赖项

- `numpy`
- `scipy`

这些库在 Google Colab 环境中已预安装。如果在其他环境中运行程序，请确保已安装这些库：

```bash
pip install numpy scipy
