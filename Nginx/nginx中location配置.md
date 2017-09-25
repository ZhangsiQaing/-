## location������

### location���﷨����
```
location [=|~|~*|^~|@] /uri/ { �� }
```

* ��=��ͷ��ʾ��ȷƥ��
* ~ ��ͷ��ʾ���ִ�Сд������ƥ��;
* ~* ��ͷ��ʾ�����ִ�Сд������ƥ��
* ^~ ��ͷ��ʾuri��ĳ�������ַ�����ͷ����������ƥ��
* / ͨ��ƥ��, ���û������ƥ��,�κ����󶼻�ƥ�䵽


### location��ƥ������ȼ�
```
(location =) > (location ����·��) > (location ^~ ·��) > (location ~,~* ����˳��) > (location ������ʼ·��) > (/)
```


## ��ƥ�����
```
location  = / {
  # ��ȷƥ�� / �����������治�ܴ��κ��ַ���
  [ configuration A ] 
}
location  / {
  # ��Ϊ���еĵ�ַ���� / ��ͷ��������������ƥ�䵽��������
  # �����������ַ���������ƥ��
  [ configuration B ] 
}
location /documents/ {
  # ƥ���κ��� /documents/ ��ͷ�ĵ�ַ��ƥ������Ժ󣬻�Ҫ������������
  # ֻ�к����������ʽû��ƥ�䵽ʱ����һ���Ż������һ��
  [ configuration C ] 
}
location ~ /documents/Abc {
  # ƥ���κ��� /documents/ ��ͷ�ĵ�ַ��ƥ������Ժ󣬻�Ҫ������������
  # ֻ�к����������ʽû��ƥ�䵽ʱ����һ���Ż������һ��
  [ configuration CC ] 
}
location ^~ /images/ {
  # ƥ���κ��� /images/ ��ͷ�ĵ�ַ��ƥ������Ժ�ֹͣ�����������򣬲�����һ����
  [ configuration D ] 
}
location ~* \.(gif|jpg|jpeg)$ {
  # ƥ�������� gif,jpg��jpeg ��β������
  # Ȼ������������ /images/ �µ�ͼƬ�ᱻ config D ������Ϊ ^~ ���ﲻ����һ������
  [ configuration E ] 
}
location /images/ {
  # �ַ�ƥ�䵽 /images/���������£��ᷢ�� ^~ ����
  [ configuration F ] 
}
location /images/abc {
  # ��ַ�ƥ�䵽 /images/abc���������£��ᷢ�� ^~ ����
  # F��G�ķ���˳����û�й�ϵ��
  [ configuration G ] 
}
location ~ /images/abc/ {
  # ֻ��ȥ�� config D ����Ч�����ƥ�� config G ��ͷ�ĵ�ַ����������������ƥ�䵽��һ�����򣬲���
    [ configuration H ] 
}
location ~* /js/.*/\.js
```
