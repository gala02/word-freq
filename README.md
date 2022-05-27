# word-freq
词频统计
def process_file(dst):  # 读文件到缓冲区
    try:  # 打开文件
        f = open(dst,'r' )  # dst为文本的目录路径
    except IOError as s:
        print(s)
        return None
    try:  # 读文件到缓冲区
        bvffer = f.read()
    except:
        print('Read File Error!')
        return None
    f.close()
    return bvffer

def process_buffer(bvffer):
    if bvffer:
        word_freq = {}                     #新建一个空字典word_freq
        # 下面添加处理缓冲区 bvffer代码，统计每个单词的频率，存放在字典word_freq
        for word in bvffer.split():      #.split()函数将bvffer切片
            if word not in word_freq:
                word_freq[word]=0
            word_freq[word]+=1
        return word_freq


def output_result(word_freq):
    if word_freq:
        sorted_word_freq = sorted(word_freq.items(), key=lambda v: v[1], reverse=True)
        for item in sorted_word_freq[:10]:  # 输出 Top 10 的单词
            print(item)

def main():
    dst = "D:/SE/Gone_with_the_wind.txt"
    bvffer = process_file(dst)
    word_freq = process_buffer(bvffer)
    output_result(word_freq)

if __name__ == "__main__":
    import cProfile
    import pstats
    cProfile.run("main()" , filename="result.out")
    p = pstats.Stats('result.out')  # 创建Stats对象
    p.sort_stats('calls').print_stats(10)  # 按调用次数排序，打印前10函数的信息
    p.strip_dirs().sort_stats("cumulative", "name").print_stats(10)  # 按运行时间和函数名进行排序，打印前10行函数的信息
    p.print_callees("process_buffer")  # 查看process_buffer()函数中调用的函数
