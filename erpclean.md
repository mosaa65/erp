# مخطط الكينونة والعلاقة (Chen ERD) المنظم للطباعة والإدراج في Word

هذا الملف يعيد **تنظيم** نفس مخطط Chen ERD الموجود في الملف الأصلي، لكن بطريقة أكثر قابلية للقراءة والإدراج داخل مستند Word.

- لا يوجد أي حذف لأي كيان أو علاقة أو خاصية.
- تم فقط **إعادة توزيع المخطط** إلى:
  1. لوحة رئيسية للعلاقات بين الكيانات.
  2. لوحات تفصيلية للخصائص حسب المجال الوظيفي.
- هذا الأسلوب يعطي شكلاً أكاديمياً أنظف ويقلل المساحة البصرية المهدورة.

## دليل الرموز

- **الكيان القوي:** مستطيل أزرق.
- **الكيان الضعيف:** مستطيل أحمر بإطار سميك.
- **العلاقة العادية:** معين ذهبي.
- **العلاقة المُعرِّفة للكيان الضعيف:** معين بني داكن بإطار سميك.
- **الخاصية العادية:** شكل بيضاوي فاتح.
- **الخاصية متعددة القيم:** بيضاوي بإطار سميك.
- **الخاصية المشتقة:** بيضاوي بخط متقطع.
- **الخاصية المركبة:** خاصية رئيسية تتفرع إلى خصائص فرعية.

---

## 1) اللوحة الرئيسية: خريطة العلاقات بين الكيانات

```mermaid
flowchart LR
    classDef entity fill:#2E86C1,stroke:#1B4F72,stroke-width:2px,color:#fff,font-weight:bold
    classDef weak_entity fill:#C0392B,stroke:#7B241C,stroke-width:4px,color:#fff,font-weight:bold
    classDef relation fill:#D68910,stroke:#9C640C,stroke-width:2px,color:#fff,font-weight:bold
    classDef weak_relation fill:#935116,stroke:#6E2C00,stroke-width:4px,color:#fff,font-weight:bold

    subgraph G1["إدارة العملاء والتأمين"]
        direction LR
        E_CLIENT["العميل"]:::entity
        R_1{"يقدم"}:::relation
        E_REQ["طلب التأمين"]:::entity
        R_9{"يولد"}:::relation
        E_POL["وثيقة التأمين"]:::entity
        R_12{"يغطي"}:::relation
        E_CLAIM["المطالبة"]:::entity
    end

    subgraph G2["هيكل المنتجات التأمينية"]
        direction TB
        E_CAT["مجال التأمين"]:::entity
        R_6{"يضم"}:::relation
        E_TYPE["نوع التأمين"]:::entity
    end

    subgraph G3["المالية والتحصيل"]
        direction TB
        E_INST["القسط المستحق"]:::weak_entity
        R_11{"يسوى بـ"}:::relation
        E_PAY["عملية السداد"]:::entity
    end

    subgraph G4["المطالبات والتتبع"]
        direction TB
        E_CDOC["مرفق المطالبة"]:::weak_entity
        E_CSTAT["سجل حالة المطالبة"]:::weak_entity
    end

    subgraph G5["الإدارة والتشغيل"]
        direction TB
        E_ROLE["الدور"]:::entity
        E_USER["حساب المستخدم"]:::entity
        E_NOTIF["الإشعار"]:::entity
        E_AUDIT["سجل العمليات"]:::entity
    end

    E_CLIENT -- "1" --- R_1
    R_1 -- "M" --- E_REQ

    E_CLIENT -- "1" --- R_2{"يملك"}:::relation
    R_2 -- "M" --- E_POL

    E_CLIENT -- "1" --- R_3{"ينشئ"}:::relation
    R_3 -- "M" --- E_CLAIM

    E_CLIENT -- "1" --- R_4{"يسدد"}:::relation
    R_4 -- "M" --- E_PAY

    E_CLIENT -- "1" --- R_5{"يستلم"}:::relation
    R_5 -- "M" --- E_NOTIF

    E_CAT -- "1" --- R_6
    R_6 -- "M" --- E_TYPE

    E_TYPE -- "1" --- R_7{"يطلب لـ"}:::relation
    R_7 -- "M" --- E_REQ

    E_TYPE -- "1" --- R_8{"يصدر كـ"}:::relation
    R_8 -- "M" --- E_POL

    E_REQ -- "1" --- R_9
    R_9 -- "1" --- E_POL

    E_POL -- "1" --- R_10{"يحتوي"}:::weak_relation
    R_10 -- "M" --- E_INST

    E_INST -- "1" --- R_11
    R_11 -- "M" --- E_PAY

    E_POL -- "1" --- R_12
    R_12 -- "M" --- E_CLAIM

    E_CLAIM -- "1" --- R_13{"يشمل"}:::weak_relation
    R_13 -- "M" --- E_CDOC

    E_CLAIM -- "1" --- R_14{"يتتبع بـ"}:::weak_relation
    R_14 -- "M" --- E_CSTAT

    E_ROLE -- "1" --- R_15{"يسند إلى"}:::relation
    R_15 -- "M" --- E_USER

    E_USER -- "1" --- R_16{"يسجل"}:::relation
    R_16 -- "M" --- E_AUDIT

    E_USER -- "1" --- R_17{"يحدث"}:::relation
    R_17 -- "M" --- E_CSTAT

    E_USER -- "1" --- R_18{"يرسل له"}:::relation
    R_18 -- "M" --- E_NOTIF
```

---

## 2) لوحة الخصائص: العميل والحسابات الإدارية

```mermaid
flowchart LR
    classDef entity fill:#2E86C1,stroke:#1B4F72,stroke-width:2px,color:#fff,font-weight:bold
    classDef attribute fill:#F8F9F9,stroke:#AAB7B8,stroke-width:1.5px,color:#222
    classDef multi_attribute fill:#F8F9F9,stroke:#424949,stroke-width:3px,color:#222

    subgraph C1["العميل"]
        direction TB
        E_CLIENT["العميل"]:::entity
        AL1(["معرف العميل (PK)"]):::attribute
        AL2(["الاسم الكامل"]):::attribute
        AL2A(["الاسم الأول"]):::attribute
        AL2B(["اسم العائلة"]):::attribute
        AL3(["الهوية"]):::attribute
        AL4(["الهاتف"]):::multi_attribute
        AL5(["البريد"]):::attribute
        AL6(["العنوان"]):::attribute
        AL6A(["المدينة"]):::attribute
        AL6B(["الشارع"]):::attribute
        AL7(["الحالة"]):::attribute
    end

    subgraph C2["حساب المستخدم"]
        direction TB
        E_USER["حساب المستخدم"]:::entity
        AU1(["معرف المستخدم - PK"]):::attribute
        AU2(["اسم المستخدم"]):::attribute
        AU3(["كلمة المرور"]):::attribute
        AU4(["الاسم"]):::attribute
        AU5(["الحالة"]):::attribute
    end

    subgraph C3["الدور"]
        direction TB
        E_ROLE["الدور"]:::entity
        AR1(["معرف الدور - PK"]):::attribute
        AR2(["اسم الدور"]):::attribute
        AR3(["الوصف"]):::attribute
    end

    subgraph C4["الإشعار"]
        direction TB
        E_NOTIF["الإشعار"]:::entity
        NO1(["معرف الإشعار - PK"]):::attribute
        NO2(["النوع"]):::attribute
        NO3(["القناة"]):::attribute
        NO4(["النص"]):::attribute
        NO5(["الحالة"]):::attribute
        NO6(["تاريخ الإرسال"]):::attribute
    end

    subgraph C5["سجل العمليات"]
        direction TB
        E_AUDIT["سجل العمليات"]:::entity
        AU_1(["معرف السجل - PK"]):::attribute
        AU_3(["الإجراء"]):::attribute
        AU_4(["الكيان"]):::attribute
        AU_5(["المفتاح/الرقم"]):::attribute
        AU_6(["ملاحظة"]):::attribute
        AU_7(["التاريخ"]):::attribute
    end

    E_CLIENT --- AL1
    E_CLIENT --- AL2
    AL2 --- AL2A
    AL2 --- AL2B
    E_CLIENT --- AL3
    E_CLIENT --- AL4
    E_CLIENT --- AL5
    E_CLIENT --- AL6
    AL6 --- AL6A
    AL6 --- AL6B
    E_CLIENT --- AL7

    E_USER --- AU1
    E_USER --- AU2
    E_USER --- AU3
    E_USER --- AU4
    E_USER --- AU5

    E_ROLE --- AR1
    E_ROLE --- AR2
    E_ROLE --- AR3

    E_NOTIF --- NO1
    E_NOTIF --- NO2
    E_NOTIF --- NO3
    E_NOTIF --- NO4
    E_NOTIF --- NO5
    E_NOTIF --- NO6

    E_AUDIT --- AU_1
    E_AUDIT --- AU_3
    E_AUDIT --- AU_4
    E_AUDIT --- AU_5
    E_AUDIT --- AU_6
    E_AUDIT --- AU_7
```

---

## 3) لوحة الخصائص: المنتجات التأمينية ودورة الإصدار

```mermaid
flowchart LR
    classDef entity fill:#2E86C1,stroke:#1B4F72,stroke-width:2px,color:#fff,font-weight:bold
    classDef attribute fill:#F8F9F9,stroke:#AAB7B8,stroke-width:1.5px,color:#222
    classDef derived_attribute fill:#F8F9F9,stroke:#7F8C8D,stroke-width:2px,color:#222,stroke-dasharray: 6 6

    subgraph P1["مجال التأمين"]
        direction TB
        E_CAT["مجال التأمين"]:::entity
        AC1(["معرف المجال - PK"]):::attribute
        AC2(["الاسم"]):::attribute
        AC3(["الوصف"]):::attribute
    end

    subgraph P2["نوع التأمين"]
        direction TB
        E_TYPE["نوع التأمين"]:::entity
        AT1(["معرف النوع - PK"]):::attribute
        AT2(["الاسم"]):::attribute
        AT3(["قواعد التغطية"]):::attribute
        AT4(["التسعير"]):::attribute
        AT5(["التحمل"]):::attribute
    end

    subgraph P3["طلب التأمين"]
        direction TB
        E_REQ["طلب التأمين"]:::entity
        AQ1(["رقم الطلب - PK"]):::attribute
        AQ2(["التاريخ"]):::attribute
        AQ3(["الحالة"]):::attribute
        AQ4(["التغطية المقترحة"]):::attribute
        AQ5(["القسط المقترح"]):::attribute
    end

    subgraph P4["وثيقة التأمين"]
        direction TB
        E_POL["وثيقة التأمين"]:::entity
        AP1(["معرف الوثيقة - PK"]):::attribute
        AP2(["رقم الوثيقة"]):::attribute
        AP3(["تاريخ الإصدار"]):::attribute
        AP4(["تاريخ البدء"]):::attribute
        AP5(["تاريخ الانتهاء"]):::attribute
        AP_DERIVED(["مدة الوثيقة"]):::derived_attribute
        AP6(["التغطية"]):::attribute
        AP7(["القسط"]):::attribute
        AP8(["الحالة"]):::attribute
    end

    E_CAT --- AC1
    E_CAT --- AC2
    E_CAT --- AC3

    E_TYPE --- AT1
    E_TYPE --- AT2
    E_TYPE --- AT3
    E_TYPE --- AT4
    E_TYPE --- AT5

    E_REQ --- AQ1
    E_REQ --- AQ2
    E_REQ --- AQ3
    E_REQ --- AQ4
    E_REQ --- AQ5

    E_POL --- AP1
    E_POL --- AP2
    E_POL --- AP3
    E_POL --- AP4
    E_POL --- AP5
    E_POL -.- AP_DERIVED
    E_POL --- AP6
    E_POL --- AP7
    E_POL --- AP8
```

---

## 4) لوحة الخصائص: المطالبات والسداد والتتبع

```mermaid
flowchart LR
    classDef entity fill:#2E86C1,stroke:#1B4F72,stroke-width:2px,color:#fff,font-weight:bold
    classDef weak_entity fill:#C0392B,stroke:#7B241C,stroke-width:4px,color:#fff,font-weight:bold
    classDef attribute fill:#F8F9F9,stroke:#AAB7B8,stroke-width:1.5px,color:#222

    subgraph F1["المطالبة"]
        direction TB
        E_CLAIM["المطالبة"]:::entity
        CL1(["معرف المطالبة - PK"]):::attribute
        CL2(["رقم المطالبة"]):::attribute
        CL3(["تاريخ الحادث"]):::attribute
        CL4(["مكان الحادث"]):::attribute
        CL5(["الوصف"]):::attribute
        CL6(["الخسارة المقدرة"]):::attribute
        CL7(["الحالة"]):::attribute
    end

    subgraph F2["مرفق المطالبة"]
        direction TB
        E_CDOC["مرفق المطالبة"]:::weak_entity
        DO1(["معرف المرفق (Partial Key)"]):::attribute
        DO2(["النوع"]):::attribute
        DO3(["الاسم"]):::attribute
        DO4(["المسار"]):::attribute
        DO5(["تاريخ الرفع"]):::attribute
    end

    subgraph F3["سجل حالة المطالبة"]
        direction TB
        E_CSTAT["سجل حالة المطالبة"]:::weak_entity
        CS1(["معرف السجل (Partial Key)"]):::attribute
        CS2(["الحالة السابقة"]):::attribute
        CS3(["الحالة الجديدة"]):::attribute
        CS4(["ملاحظة"]):::attribute
        CS5(["وقت التعديل"]):::attribute
    end

    subgraph F4["القسط المستحق"]
        direction TB
        E_INST["القسط المستحق"]:::weak_entity
        IN1(["معرف القسط (Partial Key)"]):::attribute
        IN2(["رقم القسط"]):::attribute
        IN3(["الاستحقاق"]):::attribute
        IN4(["المبلغ"]):::attribute
        IN5(["الحالة"]):::attribute
    end

    subgraph F5["عملية السداد"]
        direction TB
        E_PAY["عملية السداد"]:::entity
        PY1(["معرف الدفعة - PK"]):::attribute
        PY2(["المبلغ المسدد"]):::attribute
        PY3(["طريقة الدفع"]):::attribute
        PY4(["المرجع"]):::attribute
        PY5(["التاريخ"]):::attribute
    end

    E_CLAIM --- CL1
    E_CLAIM --- CL2
    E_CLAIM --- CL3
    E_CLAIM --- CL4
    E_CLAIM --- CL5
    E_CLAIM --- CL6
    E_CLAIM --- CL7

    E_CDOC --- DO1
    E_CDOC --- DO2
    E_CDOC --- DO3
    E_CDOC --- DO4
    E_CDOC --- DO5

    E_CSTAT --- CS1
    E_CSTAT --- CS2
    E_CSTAT --- CS3
    E_CSTAT --- CS4
    E_CSTAT --- CS5

    E_INST --- IN1
    E_INST --- IN2
    E_INST --- IN3
    E_INST --- IN4
    E_INST --- IN5

    E_PAY --- PY1
    E_PAY --- PY2
    E_PAY --- PY3
    E_PAY --- PY4
    E_PAY --- PY5
```

---

## 5) ملاحظات تنسيق مناسبة لملف Word

- عند إدراج المخطط في Word، استخدم كل لوحة كصورة مستقلة بدلاً من لقطة واحدة شديدة الاتساع.
- الأفضل وضع **اللوحة الرئيسية** أولاً، ثم اللوحات التفصيلية أسفلها كتفصيل مرجعي.
- هذا يمنحك شكلاً احترافياً قريباً من الرسومات الأكاديمية المنشورة، لكنه أوضح لأن العلاقات العامة منفصلة عن ازدحام الخصائص.

