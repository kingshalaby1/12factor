## IX. سهولة الإبعاد
### تحسين المتانة من خلال البدء السريع والإيقاف الحميد

**[عمليات](./processes) تطبيق القواعد الإثني عشر يجب أن تكون *سهلة الإبعاد*، بمعني أنه يمكنها البدء والتوقف في غضون لحظات.** هذا من شأنه تسهيل التوسع المرن والسريع، والإطلاق العاجل [للكود](./codebase) أو تغيير [متغيرات الضبط](./config)، والمتانة في إطلاقات الإنتاج.

يجب أن تسعى العمليات *لتقليل وقت البدء* قدر الإمكان. الوضع المثالي هو أن تستغرق العملية ثوانٍ قليلة من لحظة تنفيذ أمر البدء حتي لحظة تشغيل العملية وجاهزيتها لاستقبال الطلبات أو الأعمال. وقت البدء القصير يوفر مرونة أكثر لعملية [الإصدار](./build-release-run) والتوسع للأعلى، ويدعم المتانة، لأنه يُمكّن مدير العمليات من نقل العمليات إلى سيرفرات أخري بشكل أكثر سهولة متي اقتضت الحاجة.

العمليات **تتوقف بشكل حميد حينما تستقبل [إشارة وقف](http://en.wikipedia.org/wiki/SIGTERM)** من مدير العمليات. بالنسبة لعمليات الويب، فالتوقف الحميد يتم من خلال التوقف عن الإنصات إلي منفذ الخدمة (بالتالي رفض أي طلبات جديدة)، سامحةً لأي طلبات حالية بالاستكمال، ثم الخروج/التوقف التام. هذا النموذج يعتبر ضمنياً أن طلبات HTTP قصيرة (لا تستغرق أكثر من ثوانً قليلة)، أما في حالة الاستفسار المُطوّل (long polling) فإن العميل سوف يستمر بسلاسة في محاولة إعادة الإتصال متي انقطع الاتصال. 

بالنسبة للعمليات العاملة، فالتوقف الحميد يتم من خلال إعادة المهمة إلي صف انتظار الأعمال. فمثلا، في [RabbitMQ](http://www.rabbitmq.com/) تقوم العملية العاملة بإرسال إشارة [`NACK`](http://www.rabbitmq.com/amqp-0-9-1-quickref.html#basic.nack) ، في [Beanstalkd](https://beanstalkd.github.io) يتم إعادة المهمة إلى صف الانتظار تلقائياً متي انقطع اتصال العملية العاملة. النظم القائمة علي الإغلاق مثل [Delayed Job](https://github.com/collectiveidea/delayed_job#readme) تحتاج للتأكد من تحرير الإغلاق علي سجل المهمة. المفترض ضمنياً في هذا المنوذج أن كل المهام [قابلة للاستئناف](http://en.wikipedia.org/wiki/Reentrant_%28subroutine%29) وما يكفل ذلك هو طي النتائج بداخل معاملة (transaction) أو جعل العملية [معطلة عند تكرار التنفيذ](http://en.wikipedia.org/wiki/Idempotence) 

يجب أيضاً أن تكون العمليات **مرنة ضد الموت المفاجئ**، في حالة فشل الهاردوير بالرغم من كون ذلك أقل حدوثاً من التوقف الحميد من خلال إشارة وقف، إلا أنه مازال وارداً. الطريقة الموصَى بها هي استخدام قائمة انتظار داعمة مرنة، مثل Beanstalkd، التي تعيد المهمة إلي صف الانتظار متي انقطع الاتصال مع العميل أو انتهي الوقت المتاح. في الحالتين، فإن تطبيق القواعد الاثني عشر مصمم ليتعامل مع التوقفات الغير حميدة والغير متوقعة.  [Crash-only design](http://lwn.net/Articles/191059/) يأخذ هذه الفكرة إلي [خلاصتها المنطقية](http://docs.couchdb.org/en/latest/intro/overview.html).


