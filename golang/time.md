
### 时间操作

#### 获取当前时间,返回时间对象
```
now := time.Now()

# 年
year := now.Year()
# 月
mont := now.Month()
# 日
day := now.Day()
# 时
hour := now.Hour()
# 分
minute := now.Minute()
# 秒
send := now.Second()
```

#### 比较两个时间是否相等
```
now := time.Now()
time.Now().Equal(now)
```

#### 计算运行的时间(计算时间差)(纳秒数)
```
now := time.Now()
time.Now().Sub(now)
```

#### 获取当前时间戳
```
time.Now().Unix() 
time.Now().UnixNano() // 包含纳秒数
```

#### 将时间戳转为时间对象
```
time.Unix(timStamp, 0)
```

#### 时间格式化 - Go语言的时间格式化比较奇葩 2006年01月02日15时04分05秒 是固写法
```
time.Now().Format("2006-01-02 15:04:05")
```

#### 时间格式化 - 使用 GMT 日期格式表示 ( Mon, 02 Jan 2006 15:04:05 GMT )
```
time.Now().UTC().Format(http.TimeFormat)
```

#### 时间加减操作
```
# 3分钟后 
time.Now().Add(time.Second * 180).Format("2006-01-02 15:04:05")
# 3分钟前
time.Now().Add(-(time.Second * 180)).Format("2006-01-02 15:04:05")
```

#### 睡眠 1秒
```
time.Sleep(time.Second * 1)
```

#### 自定义时区
```
# 单次设置
var cstZone = time.FixedZone("CST", 8*3600) // 东八区
fmt.Println("SH : ", time.Now().In(cstZone).Format("2006-01-02 15:04:05"))

# 全局设置
func InitTime() {
	var cstZone = time.FixedZone("CST", 8*3600) // 东八
	time.Local = cstZone
}
```

#### 时间字符串转换时间
```
// time.Parse() 的默认时区是 UTC , time.Format() 的时区默认是本地
// 时间字符串转换时间戳 - 先用 time.Parse 对时间字符串进行分析，如果正确会得到一个 time.Time 对象
parse, _ := time.Parse("2006-01-02 15:04:05", "2018-01-06 16:12:00")
fmt.Println(parse.Unix()) // 2018-01-07 00:12:00

// 使用 time.ParseInLocation() 而不是 time.Parse()
localTime, _ := time.ParseInLocation("2006-01-02 15:04:05", "2018-01-06 16:12:00", time.Local)
fmt.Println(localTime.Unix()) // 2018-01-06 16:12:00
```

#### 定时器
```
func TimeTick() {

	// 定时器-每隔2秒运行一次，返回时间管道 (周期性)
	ticker := time.Tick(time.Second * 2)
	// 定时器-2秒后运行一次，返回时间管道 (单次)
	//ticker := time.After(time.Second * 2)

	for {
		select {
		case <-ticker:
			fmt.Println(time.Now().Unix())
		}
	}

}
```

#### 定时器
```
func Time_Tick() {

	// 周期性
	//timer := time.NewTicker(time.Second * 2)
	// 单次
	timer := time.NewTimer(time.Second * 2)

	go func() {

		for {
			select {
			case <-timer.C:
				fmt.Println(time.Now().Unix())
			}
		}

	}()

	// 定时器重置 (只能用于 单次 的)
	//timer.Reset(time.Second * 5)

	time.Sleep(time.Second * 2)
	// 停止定时器
	timer.Stop()

}
```
