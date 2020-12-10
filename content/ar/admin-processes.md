## XII. عمليات الإدارة
### تشغيل المهمات الإدارية كعمليات تقوم كمرة واحدة

[تشكيل العمليات](./concurrency) هو مصفوفة عمليات تستخدم للقيام بالأعمال الاعتيادية للتطبيق (مثل تلقي طلبات الويب) أثناء التشغيل. بشكل منفصل، يقوم المطور غالباً بتشغيل مهمة إدارية أو بغرض الصيانة لمرة واحدة للتطبيق، مثل:

* ترحيل قاعدة البيانات (مثل `manage.py migrate` في Django, `rake db:migrate` في Rails).
* تشغيل واجهة تحكم (تعرف أيضاً باسم واجهة [REPL](http://en.wikipedia.org/wiki/Read-eval-print_loop)) لتشغيل كود اختيارياً أو فحص النماذج في التطبيق ومقارنتها بقواعد البيانات الجارية. معظم اللغات توفر واجهة تحكم من خلال تشغيل المحرر بدون أي وسطيات إضافية (مثل `python` أو `perl`) أو في بعض الأحيان من خلال أمر منفصل (مثل `irb` في لغة Ruby، و `rails console` في Rails).
* تشغيل سكريبت لمرة واحدة يكون مرفقاً في مستودع التطبيق (مثل `php scripts/fix_bad_records.php`).

عمليات المرة الواحدة يجب أن يتم تشغيلها في بيئة مطابقة [للعمليات المعتادة](./processes) للتطبيق. يتم تشغيلها علي إصدار، باستخدام نفس [قاعدة الكود](./codebase) و [متغيرات الضبط](./config) مثلها مثل أي عمليه يتم إجراؤها علي هذا الإصدار. كود الإدارة يجب أن يتم اطلاقه مع كود التطبيق لتفادي مشاكل التزامن.

نفس طرق [عزل الإعتمادات](./dependencies) يجب اتباعها مع كل أنواع العمليات. مثلا، لو أن عملية ويب في Ruby تستخدم أمر `bundle exec thin start`. فإن ترحيل قاعدة البيانات يجب أن يستخدم `bundle exec rake db:migrate`. علي نفس النهج، برنامج python باستخدام Virtualenv يجب أن يستخدم `bin/python` لتشغيل كل من ويب سيرفر Tornado و أي عمليات إدارة `manage.py`.

القواعد الإثنا عشر تُفضّل بقوة اللغات التي توفر واجهة REPL بشكل نمطي، والتي تجعل تشغيل سكريبتات المرة الواحدة سهلاً. في الإطلاقات المحلية، يقوم المطور بتشغيل عمليات المرة الواحدة الإدارية عبر أمر مباشر في سطر الأوامر من داخل مجلد التطبيق. في إطلاقة الإنتاج، المطور يمكنه استخدام ssh أو أي آلية أخرى لتنفيذ الأوامر عن بعد توفرها بيئة التنفيذ لتشغيل هذه العملية.