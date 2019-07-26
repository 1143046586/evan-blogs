本篇文档是参考SuperAgent英文文档翻译整理成的。前段时间，在一个爬虫项目中用到了SuperAgent，因为遇到了一些坑，就详细的查阅了一下官方的文档，为了便于其他朋友查阅参考，我便对翻译的文档进行了简要整理。后期，我还会针对SuperAgent使用中遇到的一些问题进行完善，并附于文末Issue章节。同时也欢迎大家分享自己在使用SuperAgent过程中遇到的一些问题和解决方法。

### 1 简介
SuperAgent是一个轻量级、灵活的、易读的、低学习曲线的客户端请求代理模块，使用在NodeJS环境中。

官方文档：http://visionmedia.github.io/superagent

使用示例：

var request = require('superagent')

request
  .post('/api/pet')
  .send({ name: 'Manny', species: 'cat' })
  .set('X-API-Key', 'foobar')
  .set('Accept', 'application/json')
  .then(res => {
     alert('yay got ' + JSON.stringify(res.body));
  });
在后面的讲解中我们都使用request表示superagent对象，代码中也省略如下部分：

var request = require('superagent')
### 2 请求
通过调用request对象then()或end()方法，或使用await关键字，发送一个请求。

request
  .get('/search')
  .then(res => {
     // res.body, res.headers, res.status
  })
  .catch(err => {
     // err.message, err.response
  });
请求的方法类型（DELETE，HEAD，PATCH，POST和PUT）可以通过如下字符串来指定：

// 指定使用GET方法
request('GET', '/search')
  .then(function (response) {
    // success

  },  function (error) {
    // failure

});
也可以使用end()方法

request('GET', '/search')
  .end(function(error, response){
    if (response.ok) {
    }
});
可以使用完整的URLs，由于同源策略的原因，这需要服务器实现了跨域访问。

request
  .get('https://example.com/search')
  .then(res => {

  });
除了上述使用的GET方法外，也可以使用DELETE，HEAD，PATCH，POST和PUT方法，只需要简单地更改一下方法的名称。

request
  .head('/favicon.ico')
  .then(response => {

  });
在旧版本IE浏览器中delete是系统的关键字，为了兼容这一点，可以调用del()方法来避免冲突。

request
  .del('/book/1')
  .end(function(response){

  });
request默认使用GET方法发送请求，可以简写成如下：

request('/search', (error, response) => {

 });
### 3 HTTP/2
如果要使用HTTP/2协议发送请求，可以调用http2()方法。目前还暂不支持对服务器 HTTP/2能力的检测。

const request = require('superagent');
const res = await request
  .get('https://example.com/h2')
  .http2();
### 4 设置头部字段
报文header字段设置通过调用set()方法。

request
  .get('/search')
  .set('API-Key', 'foobar')
  .set('Accept', 'application/json')
  .then(callback);
你也可以传递一个对象来设置多个字段：

request
  .get('/search')
  .set({
     'API-Key': 'foobar',
     ' Accept': 'application/json' 
    })
  .then(callback);
### 5 GET请求
当我们使用GET请求传递查询字符串的时候，可以使用query()方法，同时传递一个对象作为参数，下面的代码将产生一个URL为/search?query=Manny&range=1..5&order=desc请求：

request
  .get('/search')
  .query({ query: 'Manny' })
  .query({ range: '1..5' })
  .query({ order: 'desc' })
  .then(response => {

  });
或者传递一个对象

request
  .get('/search')
  .query({
    query: 'Manny',
    range: '1..5',
    order: 'desc'
  })
  .then(response => {

  });
query()也接受一个字符串作为参数

request
  .get('/querystring')
  .query('search=Manny&range=1..5')
  .then(response => {

  })
参数拼接字符&也可以用下面的方式替代：

request
  .get('/querystring')
  .query('search=Manny')
  .query('range=1..5')
  .then(response => {

  });
### 6 HEAD请求
query()方法也可以用于HEAD请求，下面的代码将会产生一个URL为 /users?email=joe@smith.com的请求：

request
  .head('/users')
  .query({
    email: 'joe@smith.com'
  })
  .then(response => {

  });
### 7 POST / PUT请求
一个典型的JSON POST请求的代码实现如下，设置相应的Content-type头字段，再写入数据。在这个例子里，仅使用JSON字符串:

request.post('/user')
  .set('Content-Type', 'application/json')
  .send('{"name":"tj","pet":"tobi"}')
  .then(callback)
JSON成为通用的数据格式后，request对象默认Content-type类型就是application/json，因此代码就可以简化为：

request.post('/user')
  .send('{"name":"tj","pet":"tobi"}')
  .then(callback)
也可以多次调用send()方法完成

request.post('/user')
  .send({ name: 'tj' })
  .send({ pet: 'tobi' })
  .then(callback
默认情况下，发送字符串时Content-type会被设置为application/x-www-form-urlencoded，多次调用send()方法会使用&连接起来，下面的代码中最终结果为：name=tj&pet=tobi

request.post('/user')
  .send('name=tj')
  .send('pet=tobi')
  .then(callback);
SuperAgent请求的数据格式是可以扩展的，不过默认支持“form”和“json”两种格式，想要以application/x-www-form-urlencoded格式发送数据的话，则可以调用type()方法并传入'form'参数，这里默认的参数是“json”。下面的请求中，POST请求体为name=tj&pet=tobi。

request.post('/user')
  .type('form')
  .send({ name: 'tj' })
  .send({ pet: 'tobi' })
  .then(callback)
支持发送FormData对象。下面的例子中发送id="myForm"的<form>表单数据：

request.post('/user')
  .send(new FormData(document.getElementById('myForm')))
  .then(callback)
提示：FormData类型其实是在XMLHttpRequest 2级定义的，它是为序列化表以及创建与表单格式相同的数据（当然是用于XHR传输）提供便利。

### 8 Content-Type设置
通常使用set()方法

request.post('/user')
  .set('Content-Type', 'application/json')
简便的方法是调用.type()方法，传递一个规范的MIME名称，包括type/subtype，或者一个简单的后缀就像xml，json，png这样。例如:

request.post('/user')
  .type('application/json')

request.post('/user')
  .type('json')

request.post('/user')
  .type('png')
### 9 序列化请求体
SuperAgent会自动序列化JSON和表单数据，你也可以为其他类型设置自动序列化：

request.serialize['application/xml'] = function (obj) {
    return 'string generated from obj';
};

// going forward, all requests with a Content-type of
// 'application/xml' will be automatically serialized
如果你希望以自定义格式发送数据，可以通过.serialize()方法为每个请求设置相应的序列化方法，以替换内置序列化方法：

request
    .post('/user')
    .send({foo: 'bar'})
    .serialize(obj => {
        return 'string generated from obj';
    });
### 10 重发请求
当给定retry()方法时，SuperAgent将自动重试请求，如果它们以短暂的方式失败或者可能由于因特网连接不正常而失败。

这个方法有两个可选参数：重试次数（默认值3）和回调函数。它在每次重发之前调用回调(error, res)。回调可以返回true/false，以决定是否要重发请求。

request
  .get('https://example.com/search')
  .retry(2) // or:
  .retry(2, callback)
  .then(finished);
需要注意的是：应该只对HTTP等幂请求使用retry()方法（即，到达服务器的多个请求不会导致不希望的副作用，如重复购买，否则会产生多个订单）。

### 11 设置Accept
与type()方法一样，这里可以调用accept()方法来设置Accept接受类型，这个值将会被request.types所引用，支持传递一个规范的MIME名称，包括type/subtype，或者一个简单的后缀：xml，json，``png等。例如:

request.get('/user')
  .accept('application/json')

request.get('/user')
  .accept('json')

request.post('/user')
  .accept('png')
Facebook和接收JSON：如果你是在调用Facebook的API，确保请求头中Accept: application/json 。否则，Facebook 将返回Content-Type: text/javascript; charset=UTF-8响应， SuperAgent将不会解析这个响应，读取响应体会得到undefined。你需要在请求对象request加上request.accept('json')或者request.header('Accept', 'application/json')代码。

### 12 查询字符串
调用.query()方法可以自动生成查询字符串，比如向URL为?format=json&dest=/login发送一个post请求：

request
  .post('/')
  .query({ format: 'json' })
  .query({ dest: '/login' })
  .send({ post: 'data', here: 'wahoo' })
  .then(callback);
默认情况下，查询字符串的拼装是没有特定的顺序。调用sortQuery()方法将默认按照ASCII码顺序进行，你也可以传递一个比较函数来指定排序规则，比较函数需要传递两个参数（在回调过程中会自动传递进两个参数，这两个参数就是用于比较的查询字符串），返回值是正整数、负整数或者0。

// 选择默认排序
request.get('/user')
  .query('name=Nick')
  .query('search=Manny')
  .sortQuery()
  .then(callback)

// 客制化的排序函数
request.get('/user')
  .query('name=Nick')
  .query('search=Manny')
  .sortQuery((a, b) => a.length - b.length)
  .then(callback)
上述客制化的排序函数中，a和b实际上就是sortQuery()回调过程中传递进去的两个查询字符串，按照查询字符串长度来排序的，长度大的排在前面。

### 13 TLS选项
在Node.js中使用SuperAgent，支持对HTTPS请求的下列配置：

ca()：将CA证书设置为信任
cert()：设置客户端证书链
key(): 设置客户端私钥
pfx(): 设置客户端PFX或PKCS12编码的私钥和证书链
更多信息可以查看：https.request 官方文档.

var key = fs.readFileSync('key.pem'),
    cert = fs.readFileSync('cert.pem');

request
  .post('/client-auth')
  .key(key)
  .cert(cert)
  .then(callback);
var ca = fs.readFileSync('ca.cert.pem');

request
  .post('https://localhost/private-ca-server')
  .ca(ca)
  .then(res => {});
### 14 解析响应体
SuperAgent 可以解析常用的响应体，目前支持application/x-www-form-urlencoded，application/json， 和multipart/form-data格式。如下代码，你也可以对其它类型的响应体设置相应的解析方法：

// 浏览器端使用SuperAgent
request.parse['application/xml'] = function (str) {
    return {'object': 'parsed from str'};
};

// Node中使用SuperAgent
request.parse['application/xml'] = function (res, cb) {

    // 解析响应内容，设置响应体代码

    cb(null, res);
};

// 自动解析响应体

通过调用buffer(true)和parse(fn)的配合，你可以设置一个定制的解析器（定制的解析器优先于内置解析器）。 如果没有启用响应缓冲（buffer(false)），那么响应事件的触发将不会等待主体解析器完成，因此响应体将不可用。

#### 14.1 JSON/Urlencoded
response.body是解析后的内容对象，比如对于一个请求响应'{"user":{"name":"tobi"}}'字符串，response.body.user.name会返回"tobi"。同样的，与x-www-form-urlencoded格式的"user[name]=tobi"解析后值也是一样，只支持嵌套的一个层次，如果需要更复杂的数据，请使用JSON格式。

数组会以重复键名的方式进行发送，send({color: ['red','blue']})会发送"color=red&color=blue"。如果你希望数组键的名称包含"[]"，则需要自己添加，SuperAgent是不会自动添加。

#### 14.2 Multipart
Node客户端通过Formidable模块来支持multipart/form-data类型，当解析一个multipart响应时，response.files属性就可以用。假设一个请求响应得到下面的数据：

--whoop
Content-Disposition: attachment; name="image"; filename="tobi.png"
Content-Type: image/png

... data here ...
--whoop
Content-Disposition: form-data; name="name"
Content-Type: text/plain

Tobi
--whoop--
你可以获取到res.body.name名为’Tobi’，res.files.image为一个file对象，包括一个磁盘文件路径、文件名称，还有其它的文件属性。

#### 14.3 Binary
在浏览器中可以使用responseType('blob')来请求处理二进制格式的响应体。在Node.js中运行此API是不必要的。此方法的支持参数值为：

'blob'：会被传递为XmlHTTPRequest的responseType属性。
'arraybuffer'：会被传递为XmlHTTPRequest的responseType属性。
req.get('/binary.data')
  .responseType('blob')
  .then(res => {
    // res.body will be a browser native Blob type here
  });
更多的信息可以查看：xhr.responseType 文档.

### 15 响应对象属性
SuperAgent请求得到响应后，响应对象的属性主要有：

text：未解析前的响应内容，字符串类型。一般只在mime类型能够匹配"text/"，"json"，"x-www-form-urlencoding"的情况下，这个属性才会有效，默认为nodejs客户端提供；

body：响应数据解析后的对象，对象类型。

header：解析之后的响应头数据，数组类型。

type：响应报文的Content-Type值，字符串类型。

charset：响应的字符集，字符串类型。

status：响应状态标识。

statusCode：响应的状态码，数整数类型。如：200、302、404、500等。

### 15.1 text
response.text包含未解析前的响应内容，一般只在mime类型能够匹配"text/"，"json"，"x-www-form-urlencoding"的情况下，这个属性才会有效，默认为nodejs客户端提供，这是出于节省内存的考虑。因为缓冲大数据量的文本（如multipart文件或图像）的效率非常低。要强制缓冲，请参阅“响应缓冲”部分。

### 15.2 body
与SuperAgent请求数据自动序列化一样，响应数据也会自动的解析，当为一个Content-Type定义一个解析器后，就能自动解析，默认解析包含application/json和application/x-www-form-urlencoded，可以通过response.body来访问解析对象.

### 15.3 header fields
response.header包含解析之后的响应头数据，字段值都是Node处理成小写字母形式，如response.header['content-length']。

### 15.4 Content-Type
Content-Type响应头字段是一个特列，服务器提供response.type来访问它，默认response.charset是空的（如果有的话，则为指定类型），例如Content-Type值为"text/html; charset=utf8"，则response.type为text/html,response.charset为"utf8"。

### 15.5 status
响应状态标识可以用来判断请求是否成功，除此之外，可以用SuperAgent来构建理想的RESTful服务器，这些标识目前定义为:

var type = status / 100 | 0;

// status / class
res.status = status;
res.statusType = type;

// basics
res.info = 1 == type;
res.ok = 2 == type;
res.clientError = 4 == type;
res.serverError = 5 == type;
res.error = 4 == type || 5 == type;

// sugar
res.accepted = 202 == status;
res.noContent = 204 == status || 1223 == status;
res.badRequest = 400 == status;
res.unauthorized = 401 == status;
res.notAcceptable = 406 == status;
res.notFound = 404 == status;
res.forbidden = 403 == status;
### 16.中止请求
可以通过req.abort()来中止请求。

### 17.请求超时
有时网络和服务器会“卡住”，并且在接受请求后从不响应。设置超时以避免请求永远等待。

req.timeout({deadline:ms}) 或 req.timeout(ms)： ms是大于0的毫秒数。为整个请求设置一个截止时间（包括所有上传、重定向、服务器处理时间）完成。如果在那个时间内没有得到完整的响应，请求将被中止。

req.timeout({response:ms})：设置等待第一个字节从服务器到达的最大时间，但不限制整个下载可以消耗多长时间。响应超时应该比服务器响应的时间长至少几秒钟，因为它还包括进行DNS查找、TCP/IP和TLS连接的时间，以及上传请求数据的时间。

你应该同时使用deadline和response超时。通过这种方式，你可以通过较短的response超时来快速检测无响应网络，并且使用较长的deadline来在缓慢但可靠的网络上提供一个合适的时间。注意，这两个计时器都限制了上传文件的上传时间。如果上传文件，则使用长时间超时。

request
  .get('/big-file?network=slow')
  .timeout({
    response: 5000,  // Wait 5 seconds for the server to start sending,
    deadline: 60000, // but allow 1 minute for the file to finish loading.
  })
  .then(res => {
      /* responded in time */
    }, err => {
      if (err.timeout) { /* timed out! */ } else { /* other error */ }
  });
超时错误会提供一个timeout属性。

### 18.认证
Node和浏览器中都可以通过auth()方法来完成认证。

request
  .get('http://local')
  .auth('tobi', 'learnboost')
  .then(callback);
nodejs客户端也可以通过传递一个像下面这样的URL：

request.get('http://tobi:learnboost@local').then(callback);
默认在浏览器中只使用基本的认证方式，你也可以添加{type:'auto'}，以启用浏览器中内置的所有方法（摘要、NTLM等）：

request.auth('digest', 'secret', {type:'auto'})
### 19.跟随重定向
默认是向上跟随5个重定向，可以通过调用.res.redirects(n)来设置个数:

request
  .get('/some.png')
  .redirects(2)
  .then(callback);
### 20.全局状态代理
### 20.1 保存cookies
在Node SuperAgent中，默认情况下不保存cookie，但是可以使用.agent()方法创建一个含有cookie的SuperAgent副本。每个副本都有一个单独的cookie数据。

const agent = request.agent();
agent
  .post('/login')
  .then(() => {
    return agent.get('/cookied-page');
  });
在浏览器中，cookie由浏览器自动管理，因此.agent()不会格式化处理cookies。

### 20.2 多请求的默认选项
使用agent进行的多个请求中，agent调用的常规方法也将默认用于agent的后续所有的请求中。这些常规方法包括：use, on, once, set, query, type, accept, auth, withCredentials, sortQuery, retry, ok, redirects, timeout, buffer, serialize, parse, ca,  key, pfx, cert。

const agent = request.agent()
  .use(plugin)
  .auth(shared);

await agent.get('/with-plugin-and-auth');
await agent.get('/also-with-plugin-and-auth');
### 21.管道数据
在nodejs客户端中，可以使用一个请求流来传输数据。需要注意的是：用pipe()取代了end()和then()方法。

下面是将文件内容作为请求进行管道传输例子：

const request = require('superagent');
const fs = require('fs');

const stream = fs.createReadStream('path/to/my.json');
const req = request.post('/somewhere');
req.type('json');
stream.pipe(req);
注意，在管道传输到请求时，superagent使用分块传输编码发送管道数据，这不是所有服务器（例如Python WSGI服务器）都支持的。

下面是输送一个响应流到文件的管道传输例子：

const stream = fs.createWriteStream('path/to/my.json');
const req = request.get('/some.json');
req.pipe(stream);
不能将管道、回调或promise混合在一起使用，不能尝试对end()或response对象进行管道操作（pipe()），如下所示：

// 不能做下面的操作
const stream = getAWritableStream();
const req = request
  .get('/some.json')
  // BAD: this pipes garbage to the stream and fails in unexpected ways
  .end((err, this_does_not_work) => this_does_not_work.pipe(stream))
const req = request
  .get('/some.json')
  .end()
  // BAD: this is also unsupported, .pipe calls .end for you.
  .pipe(nope_its_too_late);
22.Multipart/form-data请求
superagen提供了attach() 和 field()方法用于构建multipart/form-data请求。

当你使用了field()或者attach()，你就不能使用 send()方法，你也不能设置 Content-Type （superagen会自动设置纠正后的type）。

### 22.1 附件
可以使用attach(name, [file], [options])进行文件发送。你可以通过多次调用attach()附加多个文件。attach()的参数说明如下：

name：文件名
file：文件路径字符串，或Blob/Buffer对象。
options ：（可选）字符串或自定义文件名或{filename: string}对象。在Node中也支持{contentType: 'mime/type'}。在浏览器中创建一个具有适当类型的Blob。
request
  .post('/upload')
  .attach('image1', 'path/to/felix.jpeg')
  .attach('image2', imageBuffer, 'luna.jpeg')
  .field('caption', 'My cats')
  .then(callback);
### 22.2 字段值
与HTML中表单的字段类似，你可以调用field(name,value)方法来设置字段，假设你想上传一个图片的并附带自己的名称和邮箱，那么你可以像下面这样:

 request
   .post('/upload')
   .field('user[name]', 'Tobi')
   .field('user[email]', 'tobi@learnboost.com')
   .field('friends[]', ['loki', 'jane'])
   .attach('image', 'path/to/tobi.png')
   .then(callback);
### 23.压缩
NodeJS客户端本身就提供压缩响应内容，所以你不需要做任何其它处理。

### 24.响应缓冲
为了强制缓冲response.text这样未解析前的响应内容（一般只在mime类型能够匹配"text/"，"json"，"x-www-form-urlencoding"的情况下才进行缓冲），可以调用buffer()方法，想取消默认的文本缓冲响应像text/plain, text/html这样的，可以调用buffer(false)方法。

// 强制缓冲
request
  .get('/userinfo')
  .buffer(true)
  .then(callback)

// 取消缓冲
request
  .get('/userinfo')
  .buffer(false)
  .then(callback)

当缓冲response.buffered标识有效了，那么就可以在一个回调函数里处理缓冲和未缓冲的响应.

### 25.跨域资源共享(CORS)
出于安全原因，浏览器将阻止跨源请求，除非服务器使用CORS头进行选择。浏览器还将做出额外的选项请求，以检查服务器允许的HTTP头和方法。更多信息可参阅：CORS.

withCredentials()方法可以激活发送原始cookie的能力，不过只有在Access-Control-Allow-Origin不是一个通配符(*)，并且Access-Control-Allow-Credentials为’true’的情况下才行。

request
  .get('https://api.example.com:4001/')
  .withCredentials()
  .then(res => {
    assert.equal(200, res.status);
    assert.equal('tobi', res.text);
  })
### 26.错误处理
回调函数总是会传递两个参数：error 错误和response 响应。如果没有发生错误，第一个参数将为null。

request
 .post('/upload')
 .attach('image', 'path/to/tobi.png')
 .then(response => {

 });
你可以加入监听错误的代码，错误产生之后会执行监听代码。

request
  .post('/upload')
  .attach('image', 'path/to/tobi.png')
  .on('error', handle)
  .then(response => {

  });
注意：
superagent默认情况下，对响应4xx和5xx的认为不是错误，例如当响应返回一个500或者403的时候，这些状态信息可以通过response.error，response.status和其它的响应属性来查看。

不产生响应的网络故障、超时和其他错误将不包含response.error，response.status属性。

如果希望处理404个或其他HTTP错误响应，可以查询error.status属性。当发生HTTP错误（4xx或5xx响应）时，response.error属性是error对象，这可以用来作以下检查：

if (error && error.status === 404) {
  alert('oh no ' + response.body.message);
}
else if (error) {
  // all other error types we handle generically
}
或者，可以使用ok(callback)方法来判断响应是否是错误。对ok()的回调得到响应，如果响应成功，则返回true。

request.get('/404')
  .ok(res => res.status < 500)
  .then(response => {
    // reads 404 page as a successful response
  })
### 27.进度跟踪
superagent在大型文件的上传和下载过程中触发progress事件。

request.post(url)
  .attach('field_name', file)
  .on('progress', event => {
    /* the event is:
    {
      direction: "upload" or "download"
      percent: 0 to 100 // may be missing if file size is unknown
      total: // total file size, may be missing
      loaded: // bytes downloaded or uploaded so far
    } */
  })
  .then()
event属性包括：

direction： upload 或 download
percent：0 到 100 ，文件大小未知的话为无效
total：文件总大小，可能为无效
loaded： 已经下载或上传的字节数
### 28.Promise和Generator支持
SuperAgent的请求是一个“thenable”对象，它与JavaScript承诺和异步/等待语法兼容。

如果你正在使用promises，那么就不要再调用end()或pipe()方法。任何使用then()或者await操作都会禁用所有其他使用请求的方式。

像CO或类似KOA这样的Web框架，可以在superagent使用yield：

const req = request
  .get('http://local')
  .auth('tobi', 'learnboost');
const res = yield req;
请注意，superagent期望全局promises对象存在。在Internet Explorer或Node.js 0.10中需要使用polyfill组件。

### 29.浏览器和Node版本
SuperAgent有两个实现：一个是用于web浏览器（使用XHR）的版本，另一个是用于Node.JS (使用核心的http模块)版本。

### 30.3XX处理
HTTP 3XX是重定向报文，superagent默认会自动跟踪下去，我们也不能使用redirect(0)的方法来禁止重定向，因此需要使用原生的http来处理。

request.post({
  url: '/login',
}, function (err, res, body) {
  if (!err && (res.statusCode == 301 || res.statusCode == 302)) {
    // 登录成功后服务端返回重定位报文
    cookie += ';' + res.headers['set-cookie']
    res1.redirect('listall')
  } else if (res.statusCode == 200) {
    // 登录失败服务端会返回200
    // ...
  } else {
    // 其他错误信息处理
    console.log(err)
  }
})

### 31.Issue

作者：李伯特

链接：https://www.jianshu.com/p/1432e0f29abd

來源：简书

简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。