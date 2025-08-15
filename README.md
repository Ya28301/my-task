# مهامي – Task Manager (RTL, Vanilla JS + localStorage)

تطبيق ويب خفيف لإدارة المهام باللغة العربية (اتجاه RTL) بدون أي إطار عمل.  
يدعم الإحصائيات، البحث، التصفية حسب المدة/الحالة/التصنيف، شريط تقدّم يومي، وضع ليلي/نهاري، وحفظ البيانات محليًا عبر `localStorage`.

> نسخة مباشرة (GitHub Pages):  
> **https://ya28301.github.io/my-task/**  
> *(إن لم يعمل، فعِّل Pages من **Settings → Pages**)*

---

## ✨ المزايا الحالية

- ✅ **إضافة / تعديل / حذف** المهام عبر نافذة منبثقة (Modal) + تأكيد عند الحذف
- 🔎 **بحث** بالعنوان والوصف
- 🗂️ **فلاتر**: الكل / اليوم / هذا الأسبوع / المكتملة / غير المكتملة / عمل / شخصي / دراسة
- 📊 **إحصائيات فورية**: إجمالي، مكتملة، غير مكتملة، مهام اليوم
- 📈 **شريط تقدّم يومي** (Progress bar) محسوب من نسبة المهام المكتملة
- 🌗 **وضع ليلي/نهاري** مع حفظ التفضيل في `localStorage`
- ⬇️⬆️ **استيراد/تصدير** المهام بصيغة JSON (نسخ احتياطي ومشاركة)
- 🛡️ **تقليل مخاطر XSS** باستخدام `escapeHtml` قبل عرض النصوص
- 🎨 واجهة عربية RTL وأيقونات **Font Awesome** + تنبيهات **Toastify** + حركات **animate.css**

> **ملاحظة:** لا يوجد سحب-وإفلات أو تراجع (Ctrl+Z) في النسخة الحالية.

---

## 🧱 التقنيات المستخدمة

- **HTML5, CSS3** (بدون إطار CSS)
- **JavaScript (Vanilla)**
- **CDNs**: Google Fonts، Font Awesome، animate.css، Toastify (كلها عبر HTTPS)

---

## 📁 بنية المشروع 

### أهم العناصر داخل الصفحة:
- رأس (Header) بمعلومات التطبيق.
- بطاقات إحصائيات: `#total-tasks`, `#completed-tasks`, `#pending-tasks`, `#today-tasks`.
- شريط التقدّم: `#progress-fill`, `#progress-percent`.
- البحث: حقل `#search-input` وزر `#search-btn`.
- إضافة سريعة: `#new-task-input` وزر `#add-task-btn` (يفتح المودال مُعبأً).
- فلاتر: أزرار `.filter-btn` مع `data-filter`.
- قائمة المهام: `#tasks-list` (تُبنى ديناميكيًا).
- تبديل الثيم: زر `#theme-toggle`.
- التصدير/الاستيراد: `#export-btn`, `#import-btn`, ومدخل الملف `#import-file`.
- نافذة المودال للنموذج: `#task-modal` مع الحقول (`#task-title`, `#task-description`, `#task-date`, `#task-priority`, `#task-category`).

---

## 🔄 إدارة الحالة (State)

- **المصفوفة الرئيسية**: `tasks: Task[]`
- **مفاتيح الحالة المؤقتة**:  
  `currentFilter = 'all'`, `currentSearch = ''`, `editingTaskId = null`
- **التخزين المحلي**:
  - `localStorage['tasks']` لتخزين المهام
  - `localStorage['darkMode']` لتفضيل الثيم

**شكل المهمة:**
```ts
type Task = {
  id: string;             // Date.now().toString()
  title: string;
  description?: string;
  date: string;           // YYYY-MM-DD
  priority: 'low' | 'medium' | 'high';
  category: 'work' | 'personal' | 'study';
  completed: boolean;
  createdAt: string;      // ISO timestamp
};
