## VII. ربط المنافذ
### تصدير الخدمات من خلال ربط المنافذ

يتم تنفيذ تطبيقات الويب أحياناً بداخل حاويات خوادم الويب. فمثلا تطبيقات PHP قد تعمل كوحدة بداخل [Apache HTTPD](http://httpd.apache.org/) ، كذلك تطبيقات جافا قد تعمل بداخل [Tomcat](http://tomcat.apache.org/).

**تطبيق القواعد الإثني عشر يكون قائماً بذاته بشكل كامل** ولا يعتمد علي الإقحام أثناء التشغيل لأي خادم ويب في بيئة التنفيذ بغرض إنشاء واجهة خدمية للويب. تطبيق الويب يقوم بتصدير HTTP كخدمة من خلال ربط منفذ، والإنصات للطلبات القادمة من خلال هذا المنفذ.

في بيئة التطوير المحلية، يقوم المطور بزيارة رابط خدمة مثل `http://localhost:5000/` للوصول للخدمة المقدمة من التطبيق. في حالة الإطلاق، فإن طبقة توجيه تتولي توجيه الطلبات من اسم المضيف المتاح للعامة إلي خدمات الويب المربوطة من خلال المنافذ

هذا عادة ما يتم تنفيذه باستخدام [تعريف الاعتمادات](./dependencies) لإضافة مكتبة خادم الويب إلي التطبيق، مثل [Tornado](http://www.tornadoweb.org/) مع بايثون, [Thin](http://code.macournoyer.com/thin/) مع روبي, أو [Jetty](http://www.eclipse.org/jetty/) مع جافا أو أي لغة أخري قائمة علي JVM. يحدث ذلك بالكامل في *مساحة مستخدم*، بمعني أنه بداخل كود التطبيق. التعاقد مع بيئة التنفيذ يكون علي أساس الربط مع منفذ لخدمة الطلبات.

HTTP ليس الخدمة الوحيدة التي يمكن تصديرها من خلال ربط المنافذ. تقريبا كل أنواع الخوادم يمكن تشغيلها من خلال ربط عملية مع منفذ وانتظار الطلبات عليه. الأمثلة تتضمن [ejabberd](http://www.ejabberd.im/) (يستخدم بروتوكول  [XMPP](http://xmpp.org/)), و [Redis](http://redis.io/) (يستخدم بروتوكول [Redis protocol](http://redis.io/topics/protocol)).

لاحظ أيضاً أن طريقة ربط المنافذ تعني أن أحد التطبيقات يمكن أن يكون [خدمة داعمة](./backing-services) لتطبيق آخر. من خلال تقديم رابط للخدمة الداعمة كمسمي لمورد في [متغيرات ضبط](./config) التطبيق المستهلِك.
