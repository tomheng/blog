+++
date = "2015-08-17T21:12:29+08:00"
title = "Golang中Zlib解压问题一例"
tags = ["Golang", "Zlib"]
+++

前段时间在研究git中pack协议的时候，遇到一个问题。在pack协议中有多个数据段，每个都是独立压缩的。
我们在协议中只能知道压缩数据的起点，但是却无法知道压缩数据的截止点。但是这一点对Zlib协议本身并
不会构成问题，但是因为在git的pack协议中却成了一个问题。因为一个压缩包后面可能还跟着协议数据，如果
我们无法获知压缩数据的终点也就无法找到下一段数据的起点了。

这个问题不但Golang中存在，其他的语言中也有类似的问题。之所以会出现这个问题是因为Golang在Zlib压缩中使用了
buffer，经过一番探索之后终于找到了一个解决方案。就是利用buffer.Buffered方法和Zlib中如果传入的是一个buffer，
那么就直接利用此buffer，不会再创建一个buffer。

~~~
//InflateZlib unbuffered io
func InflateZlib(r *io.SectionReader, len int) (bs []byte, err error) {
	var out bytes.Buffer
	br := bufio.NewReader(r)
	zr, err := zlib.NewReader(br)
	if err != nil {
		return
	}
	defer zr.Close()
	_, err = io.Copy(&out, zr)
	if err != nil {
		return
	}
	if out.Len() != len {
		return nil, fmt.Errorf("inflated size mismatch, expected %d, got %d", len, out.Len())
	}
	bs = out.Bytes()
	_, err = r.Seek(0-int64(br.Buffered()), 1)
	if err != nil {
		return
	}
	return
}
~~~



