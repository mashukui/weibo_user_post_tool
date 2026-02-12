# weibo_user_post_tool
用python开发的微博博主帖子采集软件，输入博主链接，即可一键爬取主页发布的所有帖子数据
> *本软件工具仅限于学术交流使用，严格遵循相关法律法规，符合平台内容合法合规性，禁止用于任何商业用途！*

# 一、背景分析
## 1.1 开发背景与功能介绍

<img width="1079" height="228" alt="weibo_slogon" src="https://github.com/user-attachments/assets/028c21f8-bfcc-461a-a766-a95acf4727b3" />

我是[@马哥python说](https://github.com/mashukui)，一枚10年+程序猿，现全职独立开发。

曾经和很多同学聊过，他们希望有一个工具，可以把微博指定用户的已发布帖子的数据采集下来，然后做数据分析使用。为了满足这类需求，我特意用python开发了这款工具：**weibo_user_post_tool**

软件运行界面：
![软件运行界面](https://files.mdnice.com/user/32110/243fe257-6c65-4197-b04b-28b69f35e41b.png)

采集结果展示：
![采集结果csv](https://files.mdnice.com/user/32110/bf28a20e-d685-4798-9358-5d18636653af.jpg)

以上。

## 1.2 软件说明

几点重要说明，请详读了解：
1. Windows系统、Mac系统均可运行
2. 软件通过接口协议爬取，并非通过模拟浏览器等RPA类工具，稳定性较高！
3. 软件运行完成后，会在当前文件夹（即，软件所在文件夹）生成csv结果文件
4. 爬取过程中，每爬一页，存一次csv。并非爬完最后一次性保存！防止因异常中断导致丢失前面的数据（每页请求间隔1~2s）
5. 爬取过程中，有log文件详细记录运行过程，方便回溯
6. 采集结果有13个字段，含：博主昵称,博主id,页码,微博id,微博bid,微博链接,发布时间,发布于,转发数,评论数,点赞数,话题标签,微博内容


# 二、主要技术
## 2.1 模块介绍
软件全部模块采用python语言开发，主要分工如下：
```python
tkinter：GUI软件界面
requests：爬虫请求
json：解析响应数据
time：间隔等待，防止反爬
pandas：保存csv结果
logging：日志记录
```
出于版权考虑，暂不公开完整源码，仅向用户提供软件使用权。

## 2.2 部分源码
软件界面：
```python
# 创建主窗口
root = tk.Tk()
root.title('爬微博博主软件v1.0 | 马哥python说')
# 设置窗口大小
root.minsize(width=850, height=660)
```
爬虫请求：
```python
# 发送请求
r = requests.get(url, headers=h1, params=params)
# 接收响应数据
json_data = r.json()
```
保存数据：
```python
# 保存数据到DF
df = pd.DataFrame(
	{
		'博主昵称': name_list,
		'博主id': user_id,
		'页码': page,
		'微博id': id_list,
		'微博bid': bid_list,
		'微博链接': wb_url_list,
		'发布时间': create_time_list,
		'发布于': region_name_list,
		'转发数': reposts_count_list,
		'评论数': comments_count_list,
		'点赞数': like_count_list,
		'话题标签': topic_list,
		'微博内容': text_list,
	}
)
# 保存csv文件
df.to_csv(self.result_file, mode='a+', index=False, header=header, encoding='utf_8_sig')
```
日志记录：
```python
def get_logger(self):
	self.logger = logging.getLogger(__name__)
	# 日志格式
	formatter = '[%(asctime)s-%(filename)s][%(funcName)s-%(lineno)d]--%(message)s'
	# 日志级别
	self.logger.setLevel(logging.DEBUG)
	# 控制台日志
	sh = logging.StreamHandler()
	log_formatter = logging.Formatter(formatter, datefmt='%Y-%m-%d %H:%M:%S')
	# info日志文件名
	info_file_name = time.strftime("%Y-%m-%d") + '.log'
	# 将其保存到特定目录
	case_dir = r'./logs/'
	info_handler = TimedRotatingFileHandler(filename=case_dir + info_file_name,
											when='MIDNIGHT',
											interval=1,
											backupCount=7,
											encoding='utf-8')
	self.logger.addHandler(sh)
	sh.setFormatter(log_formatter)
	self.logger.addHandler(info_handler)
	info_handler.setFormatter(log_formatter)
	return self.logger
```
日志文件截图：
![log文件](https://files.mdnice.com/user/32110/ed206930-2bd0-4a8a-af34-59dfcbf0623d.png)

以上。

# 三、演示视频

软件使用过程演示：[【软件演示】指定博主爬微博帖子的批量采集工具](https://mp.weixin.qq.com/s/yIj_wzkZYz6HUQqUIetiDQ)

# 四、付费说明
## 4.1 卡密说明

费用如下：
```python
日卡：使用期限1天，39元。适合试用等临时场景
月卡：使用期限1个月，149元。适合短期采集需求
季卡：使用期限3个月，399元。适合中期采集需求
年卡：使用期限1年，799元。适合长期采集需求
```
**[点击这里，自助开通！](https://mgnb.pro/product/weibo)**

## 4.2 一机一码
软件采用一机一码机制，一个卡密只能在一台电脑运行、不可多电脑运行。
## 4.3 软件多开
一台电脑仅允许运行一个软件，不支持软件多开。

## 4.4 软件维护
软件由本人独立原创开发，长期维护更新，提供稳定运行。

# 五、软件包获取

**本项目已整合到[爬微博聚合软件(weibo_one_spider)](https://github.com/mashukui/weibo_one_spider)，建议直接使用聚合版本，功能更全面、维护更及时！**

公众号"**老男孩的平凡之路**"，后台回复"**爬微博聚合软件**"获取最新软件安装包。
<img width="1938" height="364" alt="二维码-公众号放底部v2" src="https://github.com/user-attachments/assets/99164b04-d6f5-4217-979b-dc55c84856d5" />
