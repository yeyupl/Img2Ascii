<?php

/**
 * 图片转Ascii图
 * @author yeyupl@qq.com
 * @usage (new Img2Ascii('test.jpeg'))->show();
 */
class Img2Ascii {
    private $imgFile;
    private $hecData = [];

    public function __construct($imgFile) {
        $this->imgFile = $imgFile;
    }


    public function setImg($imgFile) {
        $this->imgFile = $imgFile;
        return $this;
    }

    /**
     * 图片二值化
     * @return array
     * @throws Exception
     */
    private function getHec() {
        if (!file_exists($this->imgFile)) {
            throw new \Exception('File not found!');
        }
        $res = imagecreatefromjpeg($this->imgFile);
        $size = getimagesize($this->imgFile);
        $data = array();
        for ($i = 0; $i < $size[1]; ++$i) {
            for ($j = 0; $j < $size[0]; ++$j) {
                $rgb = imagecolorat($res, $j, $i);
                $rgbarray = imagecolorsforindex($res, $rgb);
                if ($rgbarray['red'] > 120 && ($rgbarray['green'] < 80
                        || $rgbarray['blue'] < 80)
                ) {
                    $data[$i][$j] = 1;
                } else {
                    $data[$i][$j] = 0;
                }
            }
        }

        // 如果1的周围数字不为1，修改为了0
        for ($i = 0; $i < $size[1]; ++$i) {
            for ($j = 0; $j < $size[0]; ++$j) {
                $num = 0;
                if ($data[$i][$j] == 1) {
                    // 上
                    if (isset($data[$i - 1][$j])) {
                        $num = $num + $data[$i - 1][$j];
                    }
                    // 下
                    if (isset($data[$i + 1][$j])) {
                        $num = $num + $data[$i + 1][$j];
                    }
                    // 左
                    if (isset($data[$i][$j - 1])) {
                        $num = $num + $data[$i][$j - 1];
                    }
                    // 右
                    if (isset($data[$i][$j + 1])) {
                        $num = $num + $data[$i][$j + 1];
                    }
                    // 上左
                    if (isset($data[$i - 1][$j - 1])) {
                        $num = $num + $data[$i - 1][$j - 1];
                    }
                    // 上右
                    if (isset($data[$i - 1][$j + 1])) {
                        $num = $num + $data[$i - 1][$j + 1];
                    }
                    // 下左
                    if (isset($data[$i + 1][$j - 1])) {
                        $num = $num + $data[$i + 1][$j - 1];
                    }
                    // 下右
                    if (isset($data[$i + 1][$j + 1])) {
                        $num = $num + $data[$i + 1][$j + 1];
                    }
                }
                if ($num == 0) {
                    $data[$i][$j] = 0;
                }
            }
        }
        return $data;
    }

    /**
     * 显示Ascii图片
     */
    public function show() {
        $this->hecData = $this->getHec();
        foreach ($this->hecData as $val) {
            foreach ($val as $v) {
                echo $v == 1 ? 'M' : '.';
            }
            echo "\n";
        }
    }
}