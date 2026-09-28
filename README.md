# Foydalanuvchi-profili-DTO-utility-types
Mana barcha talablarni o'z ichiga olgan TypeScript moduli va uning tushuntirishi:

TypeScript
// ==========================================
// 1. Asosiy Foydalanuvchi interfeysi
// ==========================================
interface Foydalanuvchi {
    id: number;
    ism: string;
    email: string;
    parol: string;
}

// ==========================================
// 2. Partial<Foydalanuvchi> yordamida yangilash funksiyasi
// ==========================================
// Barcha maydonlarni ixtiyoriy (?) qiladi, shunda faqat o'zgarayotgan maydonni yuborish mumkin
function foydalanuvchiniYangilash(id: number, malumotlar: Partial<Foydalanuvchi>): void {
    console.log(`Foydalanuvchi (ID: ${id}) yangilandi:`, malumotlar);
}

// Ishlatilishi (faqat emailni o'zgartirish uchun):
foydalanuvchiniYangilash(1, { email: "yangi_email@gmail.com" });


// ==========================================
// 3. Readonly<Foydalanuvchi> bilan o'zgarmas obyekt
// ==========================================
const ozgarmasFoydalanuvchi: Readonly<Foydalanuvchi> = {
    id: 1,
    ism: "Sunnat",
    email: "sunnat@gmail.com",
    parol: "maxfiy123"
};

// ozgarmasFoydalanuvchi.ism = "Sardor"; 
/* 
   XATOLIK TUSHUNTIRISHI:
   TypeScript quyidagi xatoni beradi:
   Cannot assign to 'ism' because it is a read-only property.
   Sababi: 'Readonly<T>' utility type obyektning barcha maydonlarini faqat o'qish 
   uchun (readonly) qilib qo'yadi, shuning uchun ularning qiymatini keyinchalik 
   o'zgartirib bo'lmaydi.
*/


// ==========================================
// 4. Pick va Omit yordamida DTO'lar hosil qilish
// ==========================================

// Pick: Faqat kerakli maydonlarni tanlab olish (masalan, qisqa profil uchun)
type QisqaProfilDTO = Pick<Foydalanuvchi, "id" | "ism">;

const profil: QisqaProfilDTO = {
    id: 1,
    ism: "Sunnat"
};

// Omit: Ba'zi maxfiy yoki keraksiz maydonlarni chiqarib tashlash (masalan, parolsiz DTO)
type FoydalanuvchiPublicDTO = Omit<Foydalanuvchi, "parol">;

const publicUser: FoydalanuvchiPublicDTO = {
    id: 1,
    ism: "Sunnat",
    email: "sunnat@gmail.com"
    // parol maydoni bu yerda talab qilinmaydi va kiritib bo'lmaydi
};


// ==========================================
// 5. Record<K, V> yordamida kalit-qiymat obyekt turini yaratish
// ==========================================
// Masalan, har bir foydalanuvchi ID raqamiga mos keluvchi ballar ro'yxati
type FoydalanuvchiBallari = Record<number, number>;

const reyting: FoydalanuvchiBallari = {
    1: 95,
    2: 88,
    3: 100
};
Utility Types afzalliklari:
Kod takrorlanishining oldi olinadi: Yangi interfeyslarni noldan yozish o'rniga, mavjud Foydalanuvchi interfeysidan kelib chiqqan holda ularni o'zgartirib ishlatish mumkin.

Xavfsizlik: Dastur davomida o'zgarmasligi kerak bo'lgan ma'lumotlar (Readonly) yoki maxfiy maydonlar (Omit yordamida parolni yashirish) xatoshisiz boshqariladi.
