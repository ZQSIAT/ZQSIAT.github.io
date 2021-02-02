# 代码
```python
'''
1156. Swap For Longest Repeated Character Substring
Given a string text, we are allowed to swap two of the characters in the string. Find the length of the longest substring with repeated characters.
'''


class Solution:
    def maxRepOpt1(self, text: str) -> int:
        charCount = [0 for i in range(26)]
        # 计算字符串中每一种字符出现的次数
        ascii_a = ord("a")
        for c in text:
            charCount[ord(c) - ascii_a] += 1
        # print(charCount, ascii_a)
        # exit()
        # lastChar用来记录上一个出现的字符
        res, count, lastChar, length = 1, 1, text[0], len(text)
        for i in range(1, length):
            # 当前的字符与字符串中上一个出现的字符不相同的时候就需要向后边扩展看是否能够构成更长的字符
            if text[i] != lastChar:
                temp = i
                while temp + 1 < length and lastChar == text[temp + 1]:
                    temp += 1
                    count += 1
                # 避免字符间隔的情况和aaabba可以交换到前面的情况
                if charCount[ord(lastChar) - ascii_a] > count:
                    count += 1
                    if charCount[ord(lastChar) - ascii_a] > count + 1:
                        reverse_text = text[::-1]
                        return self.maxRepOpt1(reverse_text)
                        pass
                res, count = max(res, count), 1
                lastChar = text[i]
            else:
                count += 1
        # 最后处理一下字符串中字符可能全部相同的情况
        return max(count, res)


if __name__ == "__main__":
    test = Solution()
    inputS = ['aaabaaaba', 'acbaaa', 'aaa']
    print(test.maxRepOpt1(inputS[1]))
    pass
```
<br>[back to home page](./..)