## ����Pair RDDת������
������                   Ŀ��
reduceByKey(fuc)        �ϲ�������ͬ����ֵ
groupByKey()            �Ծ�����ͬ����ֵ���з���
combineByKey()   	    ʹ�ò�ͬ�ķ������ͺϲ�������ͬ����ֵ
mapValues(func)         �� pair RDD �е�ÿ��ֵӦ��һ�����������ı��
flatMapValues(func)	    �� pair RDD �е�ÿ��ֵӦ��һ�����ص������ĺ�����Ȼ��Է��ص�ÿ��Ԫ�ض�����һ����Ӧԭ���ļ�ֵ�Լ�¼��
keys()	                ����һ������������ RDD
values()	            ����һ��������ֵ�� RDD
sortByKey()	            ����һ�����ݼ������ RDD

## ����RDDת������
������	                      Ŀ��
subtractByKey(other)	    ɾ�� RDD �м��� other RDD �еļ���ͬ��Ԫ��
join(other)	                ������ RDD ����������
rightOuterJoin(other)	    ������ RDD �������Ӳ�����ȷ����һ�� RDD �ļ�������ڣ��������ӣ�
leftOuterJoin(other)	    ������ RDD �������Ӳ�����ȷ���ڶ��� RDD �ļ�������ڣ��������ӣ�
cogroup(other)	            ������ RDD ��ӵ����ͬ�������ݷ��鵽һ��


## Pair RDD�ж�����
����	                ����
countByKey()	      ��ÿ������Ӧ��Ԫ�طֱ����
collectAsMap()	      �������ӳ������ʽ���أ��Ա��ѯ
lookup(key)	          ���ظ�������Ӧ������ֵ
