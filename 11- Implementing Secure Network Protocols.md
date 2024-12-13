# Implementing Secure Network Protocols

## NETWORK ADDRESS ALLOCATION

**DHCP (Dynamic Host Configuration Protocol)**

- كل اجهزة الشبكات طبعا او ال endpoints بيكون ليهم ip address طب تعرف ازاي الجهاز بياخد ال ip ازاي؟
- المفروض اني الجهاز بياخد ال ip بشكل static يعني ازاي برضو؟
- المفروض اني ال network admin اللي فالشركه هو اللي بيقعد علي كل جهاز وبيعين ليه ip معين ودي بيبقي اسمه static
- في بقي طريقه تانيه اني الجهاز ممكن ياخد ال ip بنفسه لنفسه تلقائي ودا بنسميه dynamic وبيتم عن طريق DHCP Server
- اجهزة الشبكات سواء راوترز او فايروالز او حتي اجهزه بيتعين ليهم ip بشكل static انما لو عميل مثلا وبيستخدم خدمة الانترنت فا بياخد ip بشكل dynamic دا مش قانون يعني بس دا الشائع ف معظم الشركات
- ال DHCP دا بيكون عباره عن server متفعل عليه البروتوكول ده وبيتحدد ب range معين من الايبيهات اللي يوزعها فا لما ييجي حد وعاوز ياخد ip بشكل dynamic الجهاز بيبعت request ل DHCP Server ويرد عليه بأنه يعينلو ip و subnet mask و getway و dns

### ايه الهجمات اللي ممكن الاتاكر يستغل فيها ال DHCP؟

- **rouge DHCP**

-هنا الاتاكر بينتحل شخصيه انو DHCP ولما ال client يبعت request لل DHCP عشان ياخد اي بي فا جهاز الاتاكر هو اللي يرد عليه ب اي بي مزيف علي اساس انو ال DHCP server يعني 

-ودا بيسبب DoS او بيسبب حرمان من الخدمه اني الكلاينت مش قادر يستخدم اي بي 

![image.png](image.png)

- **DHCP starvation**

-ودا برضو نوع اتاك بيسبب DoS بس مختلفه شويه عن الاولي ازاي؟

-هنا الاتاكر بيبعت طلبات انو ياخد ip لل DHCP فا ال DHCP يرد عليه ب ip فا يبعتلو تاني ويرد عليه ويقعد يبعت كدا كتير لحد ما الايبيهات اللي ف السيرفر تخلص 

-فا ييجي الكلاينت الغلبان عاوز ياخد ip فا يبعت لل DHCP فا يرد عليه يقولو للاسف انا خلصت كل الايبيهات بتاعتي شوفلك كلبه

![image.png](image%201.png)

### حل الموضوع الattacks دي ايه؟

### DHCP Snooping

- هنا الميزه دي بتكون ف ال switch بروح مفعلها بتعملي ايه بقي؟
- اي traffic بيحصل وبيمر علي ال switch بيروح مسيف ال mac وال ip وال port بتاعه ف table طب هيوقف الهجمات ازاي اتقل هقولك
- دلوقتي لو هنعمل DHCP starvation فا لما الاتاكر يبعت طلبات لل DHCP هيبعت مره يجي فالتانيه السويتش يقول لل DHCP خلي بالك دا طلب مره ونفس الجهاز طالع من نفس البورت والايبي والماك يروح عامل denied ليه

![image.png](image%202.png)

---

## DOMAIN NAME RESOLUTION

**DNS (Domain Name System)**

- وظيفة ال DNS اني يحولي ال FQDN بتاع اي موقع لل ip address
- المفروض لما اجي افتح اي موقع زي فيسبوك مثلا المفروض اصلا اكتب ال ip بتاعه فا مش معقول اقعد احفظ ف ip فيسبوك وجوجل وتويتر وكدا هتبقي صعبه اوي
- فا ال DNS هنا بيقولك يا باشا اكتب ال domain بتاعه اللي هو [facebook.com](http://facebook.com) وانا هحولهولك ل ip وهبعتهولك تاني وتبعته للسيرفر بتاع جوجل
- ال DNS طبعا بروتوكول بيتفعل علي ال Server
- بيشتغل علي بورت 53

![image.png](image%203.png)

### ايه الهجمات اللي ممكن الاتاكر يستغل فيها ال DNS؟

- **DNS Poisoning** ⇒ دي عملية تسميم ال dns
1. **Man-in-the-Middle**
- ببساطه الاتاك دي شارحه نفسها من اسمها
- وانت بتبعت request لل DNS عشان تفتح موقع معين الاتاكر هنا بيكون معاكو ف نفس الشبكه
- وشايف ال traffic كله وانت تبعت request لل DNS وبيرد عليك ب Response فا ممكن الاتاكر ينتحل شخصية ال DNS ويديلك ip بتاع اي موقع تاني مليشيس

![image.png](image%204.png)

1. **DNS Client Cache Poisoning**
- المفروض اني كل جهاز اصلا لما تفتح موقع معين بيتخزن اسمه وال ip بتاعه في ملف اسمه HOST name:ip
- ولما تيجي تفتح موقع زي فيسبوك مثلا الجهاز بيروح للملف ده بيشوف هل ال ip موجود فيه ولا لأ ولو موجود فيه خلاص الموقع بيفتح انما لو مش موجود بيروح لل DNS
- فا ممكن الاتاكر يقدر يأكسس علي الملف ده ويعرف ip موقع بتاع فيسبوك ويغيرو بحيث لما تيجي تفتح الفيس يوديك علي موقع تاني

![image.png](image%205.png)

1. **DNS Server Cache Poisoning**
- نفس الكلام ف ال DNS Client Cache Poisoning بس ده بيكون عالسيرفر بتاع ال DHCP نفسه

![image.png](image%206.png)

### حل الموضوع الattacks دي ايه؟

### DNS Security Extensions (DNSSEC)

- دي بقي ميزه بفعلها ف ال DNS Server بتخلب التعامل بين ال client والسيرفر فالطلبات بتكون مشفره بين الاتنين
- يعني ال DNS بيروح باعت للكلاينت ال public key يروح الكلاينت باعت ال request لل DNS مشفر باستخدام ال public key يوصل للDNS يروح فاكك الشفره باستخدام ال private key

![image.png](image%207.png)

---

## HYPERTEXT TRANSFER PROTOCOL (HTTP)

### Hyper Text Transfer Protocol (HTTP)

- ودا البروتوكل الاساسي في تصفح مواقع الويب
- بيشتغل علي بورت 80
- بس مشكلته انه مش امن وسهل اختراقه ومش بيستخدم اي تشفير في نقل البيانات

### Hyper Text Transfer Protocol / SSL - TLS (HTTPS)

- طبعا عشان نلاقي طريقه كويسه في تصفح المواقع وتكون امنه ف نفس الوقت فا استخدمنا بروتوكول ال SSL اللي بقي دلوقتي اسمه TLS طب ده بيعمل ايه؟
- الSSL او TLS (في مسماها الجديد) المفروض اني البروتوكول ده بيتفعل علي السيرفر فا بيبقي امن بشكل كبير
- فكرة عمله اني كل موقع بيكون ليه شهاده بيستخرجها من CA وب public key وبيكون التعامل بيني وبين الموقع والtraffic كله encypted وامن جدا
- بيشتغل علي بورت 443

## File Transfer Protocol (FTP)

- ده بروتوكول بنستخدمه عشان نقدر ننفل الملفات او نحملها من علي النت
- علي فكره ممكن سيرفرات ال HTTP تشتغل as a FTP Server بس ده بتبقي كفائته مش كويس لو عاوز حاجه احسن وتشتغل كويس وبكفائه يبقي FTP Server كامل
- عيب البروتوكول ده انو مش امن نهائيا وال traffic بيتبعت as a plaintext فا سهل علي الاتاكر يعرف ايه الحاجه اللي ف الtraffic ده

### SSH FTP (SFTP)

- نفس البروتوكول بس ده امن جدا
- بيستخدم ال SSL فالتشفير
- بيشتغل علي بورت 22

---

## REMOTE ACCESS ARCHITECTURE

### Virtual Private Network (VPN)

- لو انا ف شبكه معينه وعاوز اعمل أكسس علي جهاز تاني مش موجود جمبي ولا حتي ف نفس دولتي طبعا انت قولت هعمل Remote Access صح كدا
- ال VPN بقي بيعمل الموضوع ده وبيخليك تدخل علي جهاز عن بعد وتأكسس عليه وانت ف مكانك وغير كدا امن جدا
- يعني مثلا انا عاوز اعمل أكسس علي سيرفرات شركتي اللي ف السعوديه وانا قاعد ف مصر فا هما بيعملولي VPN وبيحددو ليها user و pass معينين وبيدوهملي وببدأ انا بخش بيهم وبيتعملي
- طبعا في حاجه اسمها VPN Getway ودا اللي بتحميني عن انظار الاخرين اول ما الtraffic يعدي منها خلاص انا كدا بعيد عن العين وكمان اصلا ال traffic بيني وبين السيرفر اللي انا بأكسسله بيكون ف VPN Tunnel ومحدش يقدر يشوف ايه اللي جوا ال Tunnel ده
- ال VPN بيستخدم بروتوكول اسمه IPSEC

![image.png](image%208.png)

**Site-to-Site VPN**

- وده نوع VPN بيستخدم اكتر في الربط بين فروع او اكتر من شبكه لنفس الشركه ببعض
- وبيقومو مستخدمين ال VPN في نقل البيانات في ممر امن
- البيانات توصل عن ال firewalls بتروح هيا بقي متوليه الموضوع وتوصل البيانات للجهاز المعين اللي انا عاوز ابعتله ال firewall هنا قام بوظيفه ال VPN Getway

![image.png](image%209.png)

### Secure Shell (SSH)

- ودا بروتوكل برضو بستخدمه عشان اتحكم في اي جهاز عن بعد بس CLI مش GUI
- يعني ميكروسوف علي فكره موفره الميزه دي ف انظمتها اسمها RDP بتقدر تأكسس علي اي جهاز ريموتلي وتتحكم فيه وبتكون الواجهه GUI مش CLI وتتحكم فالجهاز اكنك قاعد عليه بالظبط
- هنا ال SSH بيخليني اقدر اتحكم ف الجهاز عن طريق ال commands والاوامر بس
- بيكون امن طبعا وبيستخدم ال key-based في التعامل وبرضو بتعمل يوزر وباس وبتديهم للشخص اللي هيأكسس علي الجهاز او ممكن يأكسس عن طريق key بينك وبينه من غير باسورد

---