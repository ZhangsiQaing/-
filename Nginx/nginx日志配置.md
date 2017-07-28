access_log
log_format
open_log_file_cache
log_not_found
log_subrequest
rewrite_log
error_log



日志格式允许包含的变量注释如下：
$remote_addr, $http_x_forwarded_for（反向） 记录客户端IP地址            192.168.147.1
$remote_user 记录客户端用户名称                                         
$request 记录请求的URL和HTTP协议                                        GET /tupian/mingpian-800-0-default-4-0-%E5%90%8D%E7%89%87-0_2_0_0_0-.html HTTP/1.1
$status 记录请求状态                                                    200
$body_bytes_sent 发送给客户端的字节数，不包括响应头的大小； 该变量与Apache模块mod_log_config里的“%B”参数兼容。   34536
$bytes_sent 发送给客户端的总字节数。                                     35345
$connection 连接的序列号。                                              1
$connection_requests 当前通过一个连接获得的请求数量。                     1
$msec 日志写入时间。单位为秒，精度是毫秒。                                1501231794.822
$pipe 如果请求是通过HTTP流水线(pipelined)发送，pipe值为“p”，否则为“.”。   .
$http_referer 记录从哪个页面链接访问过来的                               http://www.58pic.com/tupian/mingpian-800-0.html
$http_user_agent 记录客户端浏览器相关信息                                Mozilla/5.0 (Windows NT 6.1; WOW64; rv:54.0) Gecko/20100101 Firefox/54.02261
$request_length 请求的长度（包括请求行，请求头和请求正文）。               2245
$request_time 请求处理时间，单位为秒，精度毫秒； 从读入客户端的第一个字节开始，直到把最后一个字符发送给客户端后进行日志写入为止。   2.117
$time_iso8601 ISO8601标准格式下的本地时间。        2017-07-28T02:04:59-07:00
$time_local 通用日志格式下的本地时间。              28/Jul/2017:02:04:59 -0700