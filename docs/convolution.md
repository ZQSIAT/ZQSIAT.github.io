# 代码
```
"""
sensetime 面试深度学习基础，二维卷积的python实现
2021-02-01
"""
import numpy as np
import matplotlib.pyplot as plt


class Convolution:
    def get_padding(self, ks, mode="SAME"):  #
        """
            Return padding list in different modes.
            params: inputs (input array)
            params: mode: "SAME", "FULL", "VALID"
            params: ks (kernel size) [p, q]
            return: padding list [n,m,j,k]
        """
        pad = None
        if mode == "SAME":
            pad = [(ks[0] - 1) // 2, (ks[1] - 1) // 2, (ks[0] - 1) // 2, (ks[1] - 1) // 2]
            if ks[0] % 2 == 0:
                pad[2] += 1
            if ks[1] % 2 == 0:
                pad[3] += 1
            pass
        elif mode == "VALID":
            pad = [0, 0, 0, 0]
            pass
        elif mode == "FULL":
            pad = [ks[0] - 1, ks[1] - 1, ks[0] - 1, ks[1] - 1]
            pass
        else:
            raise RuntimeError("Invalid mode!")
            pass
        return pad
        pass

    def conv(self, inputs, kernel, stride, mode="SAME"):
        outputs = None
        ks = kernel.shape[:2]
        # get padding
        pad = self.get_padding(ks, mode)
        padded_inputs = np.pad(inputs, pad_width=((pad[0], pad[2]), (pad[1], pad[3]), (0, 0)), mode="constant")
        # a = padded_inputs[0:0 + ks[1], 0:0 + ks[0], :]
        # b = a * kernel
        # print(a, b)
        # print(a.shape, b.shape)
        # exit()
        height, width, channels = inputs.shape
        out_width = int((width + pad[0] + pad[2] - ks[0]) / stride + 1)
        out_height = int((height + pad[1] + pad[3] - ks[1]) / stride + 1)
        outputs = np.empty(shape=(out_height, out_width, channels))
        for r, y in enumerate(range(0, padded_inputs.shape[0] - ks[1] + 1, stride)):
            for c, x in enumerate(range(0, padded_inputs.shape[1] - ks[0] + 1, stride)):
                outputs[r][c][:] = np.sum(padded_inputs[y:y + ks[1], x:x + ks[0], :] * kernel)
                pass
            pass
        return outputs
        pass

    pass


if __name__ == "__main__":
    inputs = np.random.randint(0, 255, size=(224, 224, 3))
    kernel = np.array([[1, 1, 1], [0, 0, 0], [-1, -1, -1]])
    stride = 1
    test = Convolution()

    outputs = test.conv(inputs, kernel, stride, mode="SAME")
    plt.imshow(outputs)
    plt.show()
    print('input: \n', inputs[:, :, 0], '\n', inputs.shape, '\n')
    print('output: \n', outputs[:, :, 0], '\n', outputs.shape)
    pass
```
<br>[back to home page](./..)