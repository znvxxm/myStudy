##����
1.��ô��spark���̴�������ʲô���ӵģ�

pom�ļ���������������ͬ�Ĳ�����Դ�ɲ�ͬ�İ�
���õĴ������������ǰ��������ģ���Ҫ���ٶ�һ��������ȥ
1���ݰ�  ֻ�ǹ��̵�class�ļ�������������jar
2���ְ�  ����class�ļ����������������ʽһ����.tar.gz,��ѹ��������jar��lib��

ע�⣺�����Ǻ��ִ����ʽ�����������Ҫ����°��Ƿ��Ѿ����
�ݰ���ͨ�����xx.jar--�Ҽ��򿪷�ʽ--��ѹ��--���Ƿ���class�ļ�
�ְ�����ѹ�������ļ�Ŀ¼���Ƿ�����Ӧ������



2.�ڷ������ϵ��ύ������ʲô��
�ڽ�spark���̴��jar�����ϴ�����Ҫ�õ���������spark-submit
ע�⣺spark1.0 ��spark-submit  spark2.0 ��spark2-submit


�ύ����Ϊ��
spark2-submit \
--name SparkSqlSchema \
--master local[*] \
--class SparkSqlSchema \
--jars /home/bdp_stream_power/jars/lib/spark-sql_2.11-2.3.0.jar \
/home/bdp_stream_power/jars/src/src-beta-2.0/SparkTest/sparksql04-1.0-SNAPSHOT.jar



--nameΪָ���������·������idea��ѡ����--�Ҽ�--Copy_Refrence  ���ɵõ����ڰ��е�λ��
--jars Ϊ����������jar������Ҫָ�������Ҫ��������·�������jar��֮���ö��Ÿ���
���һ��Ϊ����jar,ǰ�治��Ҫ���κα���ǰ׺


##�������ύ����
1.ZNV_E_SparkStreaming_Application��д��Ӧ�Ĺ�����
2.���ص�����ɺ�ֱ�Ӵ��
3.�ڷ�������Ӧ��·���½���Ŀ¼
  ��KafkaStreaming�࣬��Ҫ����·������Ŀ¼/home/bdp_stream_power/jars/src/src-beta-2.0/KafkaStreaming
4.ZNV_E_SparkStreaming_Application-1.0-SNAPSHOT.jar�ϴ����ϲ�������Ŀ¼��
5.��/home/bdp_stream_power/toolsĿ¼�½��������������ű����޸������Ӧ�Ĳ���
