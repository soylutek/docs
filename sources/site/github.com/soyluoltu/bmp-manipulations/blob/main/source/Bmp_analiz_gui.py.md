# Source: https://github.com/soyluoltu/bmp-manipulations/blob/main/source/Bmp_analiz_gui.py

[soyluoltu](https://github.com/soyluoltu) / **[bmp-manipulations](https://github.com/soyluoltu/bmp-manipulations)** Public

- [Notifications](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations)
- [Star 0](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations)
 

 

## FilesExpand file tree

 main

/

# Bmp\_analiz\_gui.py

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

## History

[History](https://github.com/soyluoltu/bmp-manipulations/commits/main/source/Bmp_analiz_gui.py)

History

557 lines (430 loc) · 22.8 KB

 main

/

# Bmp\_analiz\_gui.py

Copy path

Top

## File metadata and controls

- Code
 
- Blame
 

557 lines (430 loc) · 22.8 KB

[Raw](https://github.com/soyluoltu/bmp-manipulations/raw/refs/heads/main/source/Bmp_analiz_gui.py)

Copy raw file

Download raw file

Open symbols panel

Edit and raw actions

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

37

38

39

40

41

42

43

44

45

46

47

48

49

50

51

52

53

54

55

56

57

58

59

60

61

62

63

64

65

66

67

68

69

70

71

72

73

74

75

76

77

78

79

80

81

82

83

84

85

86

87

88

89

90

91

92

93

94

95

96

97

98

99

100

101

102

103

104

105

106

107

108

109

110

111

112

113

114

115

116

117

118

119

120

121

122

123

124

125

126

127

128

129

130

131

132

133

134

135

136

137

138

139

140

141

142

143

144

145

146

147

148

149

150

151

152

153

154

155

156

157

158

159

160

161

162

163

164

165

166

167

168

169

170

171

172

173

174

175

176

177

178

179

180

181

182

183

184

185

186

187

188

189

190

191

192

193

194

195

196

197

198

199

200

201

202

203

204

205

206

207

208

209

210

211

212

213

214

215

216

217

218

219

220

221

222

223

224

225

226

227

228

229

230

231

232

233

234

235

236

237

238

239

240

241

242

243

244

245

246

247

248

249

250

251

252

253

254

255

256

257

258

259

260

261

262

263

264

265

266

267

268

269

270

271

272

273

274

275

276

277

278

279

280

281

282

283

284

285

286

287

288

289

290

291

292

293

294

295

296

297

298

299

300

301

302

303

304

305

306

307

308

309

310

311

312

313

314

315

316

317

318

319

320

321

322

323

324

325

326

327

328

329

330

331

332

333

334

335

336

337

338

339

340

341

342

343

344

345

346

347

348

349

350

351

352

353

354

355

356

357

358

359

360

361

362

363

364

365

366

367

368

369

370

371

372

373

374

375

376

377

378

379

380

381

382

383

384

385

386

387

388

389

390

391

392

393

394

395

396

397

398

399

400

401

402

403

404

405

406

407

408

409

410

411

412

413

414

415

416

417

418

419

420

421

422

423

424

425

426

427

428

429

430

431

432

433

434

435

436

437

438

439

440

441

442

443

444

445

446

447

448

449

450

451

452

453

454

455

456

457

458

459

460

461

462

463

464

465

466

467

468

469

470

471

472

473

474

475

476

477

478

479

480

481

482

483

484

485

486

487

488

489

490

491

492

493

494

495

496

497

498

499

500

501

502

503

504

505

506

507

508

509

510

511

512

513

514

515

516

517

518

519

520

521

522

523

524

525

526

527

528

529

530

531

532

533

534

535

536

537

538

539

540

541

542

543

544

545

546

547

548

549

550

551

552

553

554

555

556

557

import tkinter as tk

from tkinter import filedialog, ttk, messagebox, scrolledtext

import struct

import os

import time

from collections import Counter

import threading

from PIL import Image, ImageTk

import matplotlib.pyplot as plt

from matplotlib.backends.backend\_tkagg import FigureCanvasTkAgg

import numpy as np

class BMPAnalyzerApp:

def \_\_init\_\_(self, root):

self.root \= root

self.root.title("BMP Dosya & Renk Analiz Programı")

self.root.geometry("1024x768")

self.root.configure(bg\="#f0f0f0")

\# Uygulama simgesi (opsiyonel)

\# self.root.iconbitmap("icon.ico")

\# Değişkenler

self.file\_path \= tk.StringVar()

self.status\_text \= tk.StringVar()

self.status\_text.set("Hazır. Lütfen bir BMP dosyası seçin.")

self.image\_data \= None

self.analysis\_result \= None

self.color\_distribution \= None

\# Ana çerçeve

self.main\_frame \= ttk.Frame(root)

self.main\_frame.pack(fill\="both", expand\=True, padx\=10, pady\=10)

\# Üst panel - Dosya seçimi

self.file\_frame \= ttk.LabelFrame(self.main\_frame, text\="BMP Dosyası")

self.file\_frame.pack(fill\="x", padx\=5, pady\=5)

self.file\_entry \= ttk.Entry(self.file\_frame, textvariable\=self.file\_path, width\=70)

self.file\_entry.pack(side\="left", padx\=5, pady\=5, fill\="x", expand\=True)

self.browse\_button \= ttk.Button(self.file\_frame, text\="Dosya Seç", command\=self.browse\_file)

self.browse\_button.pack(side\="left", padx\=5, pady\=5)

self.analyze\_button \= ttk.Button(self.file\_frame, text\="Analiz Et", command\=self.start\_analysis)

self.analyze\_button.pack(side\="left", padx\=5, pady\=5)

\# Orta panel - Analiz sonuçları

self.notebook \= ttk.Notebook(self.main\_frame)

self.notebook.pack(fill\="both", expand\=True, padx\=5, pady\=5)

\# Sekme 1: Görüntü ve Temel Bilgiler

self.info\_frame \= ttk.Frame(self.notebook)

self.notebook.add(self.info\_frame, text\="Görüntü ve Bilgiler")

\# Görüntü ve Bilgiler için yatay düzen

self.info\_pane \= ttk.PanedWindow(self.info\_frame, orient\="horizontal")

self.info\_pane.pack(fill\="both", expand\=True, padx\=5, pady\=5)

\# Sol taraf - Görüntü

self.image\_frame \= ttk.LabelFrame(self.info\_pane, text\="BMP Görüntüsü")

self.info\_pane.add(self.image\_frame, weight\=1)

self.image\_label \= ttk.Label(self.image\_frame)

self.image\_label.pack(fill\="both", expand\=True, padx\=5, pady\=5)

\# Sağ taraf - Temel Bilgiler

self.details\_frame \= ttk.LabelFrame(self.info\_pane, text\="Dosya Bilgileri")

self.info\_pane.add(self.details\_frame, weight\=1)

self.details\_text \= scrolledtext.ScrolledText(self.details\_frame, width\=40, height\=15, wrap\=tk.WORD)

self.details\_text.pack(fill\="both", expand\=True, padx\=5, pady\=5)

self.details\_text.config(state\="disabled")

\# Sekme 2: Renk Analizi

self.color\_frame \= ttk.Frame(self.notebook)

self.notebook.add(self.color\_frame, text\="Renk Analizi")

\# Renk analizi için yatay düzen

self.color\_pane \= ttk.PanedWindow(self.color\_frame, orient\="horizontal")

self.color\_pane.pack(fill\="both", expand\=True, padx\=5, pady\=5)

\# Sol taraf - Renk Dağılımı Grafiği

self.chart\_frame \= ttk.LabelFrame(self.color\_pane, text\="Renk Dağılımı")

self.color\_pane.add(self.chart\_frame, weight\=2)

\# Renk grafiği için placeholder

self.figure \= plt.Figure(figsize\=(5, 4), dpi\=100)

self.canvas \= FigureCanvasTkAgg(self.figure, self.chart\_frame)

self.canvas.get\_tk\_widget().pack(fill\="both", expand\=True)

\# Sağ taraf - En Çok Kullanılan Renkler Listesi

self.color\_list\_frame \= ttk.LabelFrame(self.color\_pane, text\="En Çok Kullanılan Renkler")

self.color\_pane.add(self.color\_list\_frame, weight\=1)

\# Renk listesi için scrollbar

self.color\_list \= ttk.Treeview(self.color\_list\_frame, columns\=("color", "count", "percent"), show\="headings")

self.color\_list.heading("color", text\="RGB Değeri")

self.color\_list.heading("count", text\="Piksel Sayısı")

self.color\_list.heading("percent", text\="Yüzde (%)")

self.color\_list.column("color", width\=100)

self.color\_list.column("count", width\=80)

self.color\_list.column("percent", width\=80)

scrollbar \= ttk.Scrollbar(self.color\_list\_frame, orient\="vertical", command\=self.color\_list.yview)

self.color\_list.configure(yscrollcommand\=scrollbar.set)

scrollbar.pack(side\="right", fill\="y")

self.color\_list.pack(side\="left", fill\="both", expand\=True)

\# Durum çubuğu

self.status\_bar \= ttk.Label(root, textvariable\=self.status\_text, relief\="sunken", anchor\="w")

self.status\_bar.pack(side\="bottom", fill\="x")

\# İleri düzey özellikler için menü

self.create\_menu()

def create\_menu(self):

menubar \= tk.Menu(self.root)

\# Dosya menüsü

file\_menu \= tk.Menu(menubar, tearoff\=0)

file\_menu.add\_command(label\="Aç", command\=self.browse\_file)

file\_menu.add\_command(label\="Analiz Et", command\=self.start\_analysis)

file\_menu.add\_separator()

file\_menu.add\_command(label\="Çıkış", command\=self.root.quit)

menubar.add\_cascade(label\="Dosya", menu\=file\_menu)

\# Analiz menüsü

analyze\_menu \= tk.Menu(menubar, tearoff\=0)

analyze\_menu.add\_command(label\="Renk Analizi", command\=self.show\_color\_analysis)

analyze\_menu.add\_command(label\="Rapor Oluştur", command\=self.create\_report)

menubar.add\_cascade(label\="Analiz", menu\=analyze\_menu)

\# Yardım menüsü

help\_menu \= tk.Menu(menubar, tearoff\=0)

help\_menu.add\_command(label\="Hakkında", command\=self.show\_about)

menubar.add\_cascade(label\="Yardım", menu\=help\_menu)

self.root.config(menu\=menubar)

def browse\_file(self):

file\_path \= filedialog.askopenfilename(

title\="BMP Dosyası Seç",

filetypes\=\[("BMP Dosyaları", "\*.bmp"), ("Tüm Dosyalar", "\*.\*")\]

)

if file\_path:

self.file\_path.set(file\_path)

self.status\_text.set(f"Dosya seçildi: {os.path.basename(file\_path)}")

\# Dosya uzantısını kontrol et

if not file\_path.lower().endswith('.bmp'):

messagebox.showwarning(

"Dosya Türü Uyarısı",

"Seçilen dosya '.bmp' uzantılı değil. Dosya bir BMP dosyası olmayabilir."

)

\# Seçilen dosyayı göster

self.load\_image\_preview()

def load\_image\_preview(self):

try:

file\_path \= self.file\_path.get()

if not file\_path:

return

\# PIL ile görüntüyü yükle

img \= Image.open(file\_path)

\# Görüntüyü uygun boyuta getir

width, height \= img.size

max\_size \= 400

if width \> max\_size or height \> max\_size:

ratio \= min(max\_size / width, max\_size / height)

new\_width \= int(width \* ratio)

new\_height \= int(height \* ratio)

img \= img.resize((new\_width, new\_height), Image.LANCZOS)

\# Tkinter için görüntüyü hazırla

self.image\_data \= ImageTk.PhotoImage(img)

self.image\_label.config(image\=self.image\_data)

\# Temel dosya bilgilerini göster

file\_size \= os.path.getsize(file\_path)

file\_size\_kb \= file\_size / 1024

file\_size\_mb \= file\_size\_kb / 1024

file\_info \= (

f"Dosya: {os.path.basename(file\_path)}\\n"

f"Boyut: {file\_size:,} byte"

)

if file\_size\_kb \>= 1:

file\_info += f" ({file\_size\_kb:.2f} KB)"

if file\_size\_mb \>= 1:

file\_info += f" ({file\_size\_mb:.2f} MB)"

file\_info += f"\\nGenişlik: {width} piksel\\nYükseklik: {height} piksel\\n"

self.update\_details\_text(file\_info)

except Exception as e:

messagebox.showerror("Hata", f"Görüntü yüklenirken bir hata oluştu:\\n{str(e)}")

self.status\_text.set("Hata: Görüntü yüklenemedi.")

def update\_details\_text(self, text):

self.details\_text.config(state\="normal")

self.details\_text.delete(1.0, tk.END)

self.details\_text.insert(tk.END, text)

self.details\_text.config(state\="disabled")

def start\_analysis(self):

file\_path \= self.file\_path.get()

if not file\_path:

messagebox.showinfo("Bilgi", "Lütfen önce bir BMP dosyası seçin.")

return

\# Analizi arka planda çalıştır

self.status\_text.set("Analiz yapılıyor... Lütfen bekleyin.")

self.analyze\_button.config(state\="disabled")

\# Analizi ayrı bir thread'de başlat

thread \= threading.Thread(target\=self.run\_analysis, daemon\=True)

thread.start()

def run\_analysis(self):

try:

file\_path \= self.file\_path.get()

start\_time \= time.time()

\# BMP analizi

result \= self.analyze\_bmp\_colors(file\_path)

if result:

total\_pixels, unique\_color\_count, color\_distribution \= result

self.analysis\_result \= result

self.color\_distribution \= color\_distribution

\# GUI güncelleme işlemleri ana thread'de yapılmalı

self.root.after(0, lambda: self.update\_analysis\_results(total\_pixels, unique\_color\_count, color\_distribution))

elapsed\_time \= time.time() \- start\_time

self.status\_text.set(f"Analiz tamamlandı ({elapsed\_time:.2f} saniye). {unique\_color\_count:,} benzersiz renk bulundu.")

else:

self.status\_text.set("Analiz sırasında bir hata oluştu.")

except Exception as e:

self.root.after(0, lambda: messagebox.showerror("Analiz Hatası", f"Analiz sırasında bir hata oluştu:\\n{str(e)}"))

self.status\_text.set("Hata: Analiz tamamlanamadı.")

finally:

self.root.after(0, lambda: self.analyze\_button.config(state\="normal"))

def update\_analysis\_results(self, total\_pixels, unique\_color\_count, color\_distribution):

\# Dosya bilgilerini güncelle

file\_path \= self.file\_path.get()

try:

\# BMP header bilgilerini oku

width, height, data\_offset, bit\_depth \= self.read\_bmp\_header(file\_path)

file\_size \= os.path.getsize(file\_path)

\# Bilgileri güncelle

file\_info \= (

f"Dosya: {os.path.basename(file\_path)}\\n"

f"Boyut: {file\_size:,} byte ({file\_size/1024:.2f} KB)\\n"

f"Genişlik: {width} piksel\\n"

f"Yükseklik: {abs(height)} piksel\\n"

f"Bit derinliği: {bit\_depth} bit\\n"

f"Toplam piksel sayısı: {total\_pixels:,}\\n"

f"Benzersiz renk sayısı: {unique\_color\_count:,}\\n"

f"Renk çeşitliliği oranı: {unique\_color\_count/total\_pixels\*100:.2f}%\\n"

)

self.update\_details\_text(file\_info)

\# Renk analizini güncelle

self.update\_color\_analysis(total\_pixels, color\_distribution)

\# Renk analizi sekmesine geç

self.notebook.select(1)

except Exception as e:

messagebox.showerror("Hata", f"Sonuçlar güncellenirken hata oluştu:\\n{str(e)}")

def update\_color\_analysis(self, total\_pixels, color\_distribution):

\# Renk listesini temizle

for item in self.color\_list.get\_children():

self.color\_list.delete(item)

\# En çok kullanılan 100 rengi listele

for i, (color, count) in enumerate(color\_distribution.most\_common(100)):

percent \= count / total\_pixels \* 100

if isinstance(color, tuple) and len(color) \>= 3:

color\_str \= f"RGB{color\[:3\]}"

else:

color\_str \= f"İndeks: {color}"

self.color\_list.insert("", "end", values\=(color\_str, f"{count:,}", f"{percent:.4f}%"))

\# İlk rengi seç

if i \== 0:

self.color\_list.selection\_set(self.color\_list.get\_children()\[0\])

\# Renk dağılımını görselleştir

self.draw\_color\_chart(color\_distribution, total\_pixels)

def draw\_color\_chart(self, color\_distribution, total\_pixels):

self.figure.clear()

\# En çok kullanılan 10 renk için pasta grafiği

ax \= self.figure.add\_subplot(111)

top\_colors \= color\_distribution.most\_common(10)

others\_count \= sum(count for \_, count in color\_distribution.most\_common()\[10:\])

if others\_count \> 0:

top\_colors.append(("Diğer Renkler", others\_count))

labels \= \[\]

sizes \= \[\]

colors\_rgb \= \[\]

for color, count in top\_colors:

if color \== "Diğer Renkler":

labels.append(color)

colors\_rgb.append("#CCCCCC") \# Gri renk

else:

if isinstance(color, tuple) and len(color) \>= 3:

labels.append(f"RGB{color\[:3\]}")

\# RGB değerlerini 0-1 aralığına normalize et

r, g, b \= \[c/255 for c in color\[:3\]\]

colors\_rgb.append((r, g, b))

else:

labels.append(f"İndeks: {color}")

colors\_rgb.append("#CCCCCC") \# Gri renk

sizes.append(count)

\# Pasta dilimlerini yüzde olarak göster

sizes\_percent \= \[s / total\_pixels \* 100 for s in sizes\]

\# Pasta grafiği çiz

wedges, texts, autotexts \= ax.pie(

sizes\_percent,

labels\=None,

autopct\='%1.1f%%',

startangle\=90,

colors\=colors\_rgb

)

\# Grafik başlığı ve ayarları

ax.set\_title('En Çok Kullanılan 10 Renk')

ax.axis('equal') \# Dairesel görünüm için

\# Renk etiketlerini düzenle

legend \= ax.legend(wedges, labels, title\="Renkler", loc\="center left", bbox\_to\_anchor\=(1, 0, 0.5, 1))

self.figure.tight\_layout()

self.canvas.draw()

def show\_color\_analysis(self):

if not self.color\_distribution:

messagebox.showinfo("Bilgi", "Lütfen önce bir BMP dosyası analiz edin.")

return

self.notebook.select(1) \# Renk analizi sekmesine geç

def create\_report(self):

if not self.analysis\_result:

messagebox.showinfo("Bilgi", "Lütfen önce bir BMP dosyası analiz edin.")

return

\# Rapor kaydetme dosya diyalogu

file\_path \= filedialog.asksaveasfilename(

title\="Raporu Kaydet",

defaultextension\=".txt",

filetypes\=\[("Metin Dosyaları", "\*.txt"), ("Tüm Dosyalar", "\*.\*")\]

)

if not file\_path:

return

try:

total\_pixels, unique\_color\_count, color\_distribution \= self.analysis\_result

bmp\_path \= self.file\_path.get()

\# BMP header bilgilerini oku

width, height, data\_offset, bit\_depth \= self.read\_bmp\_header(bmp\_path)

file\_size \= os.path.getsize(bmp\_path)

with open(file\_path, 'w', encoding\='utf-8') as f:

f.write("BMP DOSYA ANALİZ RAPORU\\n")

f.write("=" \* 50 + "\\n\\n")

f.write(f"Analiz Tarihi: {time.strftime('%d.%m.%Y %H:%M:%S')}\\n\\n")

f.write("DOSYA BİLGİLERİ\\n")

f.write("-" \* 30 + "\\n")

f.write(f"Dosya Adı: {os.path.basename(bmp\_path)}\\n")

f.write(f"Dosya Yolu: {bmp\_path}\\n")

f.write(f"Dosya Boyutu: {file\_size:,} byte ({file\_size/1024:.2f} KB)\\n")

f.write(f"Genişlik: {width} piksel\\n")

f.write(f"Yükseklik: {abs(height)} piksel\\n")

f.write(f"Bit Derinliği: {bit\_depth} bit\\n\\n")

f.write("RENK ANALİZİ\\n")

f.write("-" \* 30 + "\\n")

f.write(f"Toplam Piksel Sayısı: {total\_pixels:,}\\n")

f.write(f"Benzersiz Renk Sayısı: {unique\_color\_count:,}\\n")

f.write(f"Renk Çeşitliliği Oranı: {unique\_color\_count/total\_pixels\*100:.2f}%\\n\\n")

f.write("EN ÇOK KULLANILAN RENKLER\\n")

f.write("-" \* 30 + "\\n")

f.write(f"{'Renk':<20} {'Piksel Sayısı':<15} {'Yüzde (%)':<10}\\n")

f.write("-" \* 50 + "\\n")

for i, (color, count) in enumerate(color\_distribution.most\_common(20)):

percent \= count / total\_pixels \* 100

if isinstance(color, tuple) and len(color) \>= 3:

color\_str \= f"RGB{color\[:3\]}"

else:

color\_str \= f"İndeks: {color}"

f.write(f"{color\_str:<20} {count:<15,} {percent:<10.4f}\\n")

messagebox.showinfo("Bilgi", f"Rapor başarıyla kaydedildi:\\n{file\_path}")

except Exception as e:

messagebox.showerror("Hata", f"Rapor oluşturulurken bir hata oluştu:\\n{str(e)}")

def show\_about(self):

about\_text \= (

"BMP Dosya Analiz Programı\\n\\n"

"Bu program BMP dosyalarını analiz ederek dosyadaki benzersiz renkleri "

"ve piksel dağılımlarını tespit eder.\\n\\n"

"Geliştirilme Tarihi: Nisan 2025"

)

messagebox.showinfo("Hakkında", about\_text)

def read\_bmp\_header(self, file\_path):

"""

BMP dosyasının header bilgilerini okur ve görüntü boyutlarını döndürür.

Args:

file\_path (str): BMP dosyasının yolu

Returns:

tuple: (genişlik, yükseklik, veri başlangıç offseti, bit derinliği)

"""

with open(file\_path, 'rb') as f:

\# BMP header (14 bytes)

header \= f.read(14)

if len(header) < 14:

raise ValueError("Geçersiz BMP dosyası: Header eksik")

\# BMP imzasını kontrol et

if header\[0:2\] != b'BM':

raise ValueError("Geçersiz BMP dosyası: BM imzası bulunamadı")

\# Veri offsetini al (piksel verisinin başladığı yer)

data\_offset \= struct.unpack('<I', header\[10:14\])\[0\]

\# DIB header boyutu bilgisini al

dib\_header\_size \= struct.unpack('<I', f.read(4))\[0\]

\# DIB header'ı oku (boyutu değişebilir)

f.seek(14) \# BMP header sonrasına git

dib\_header \= f.read(dib\_header\_size)

\# Görüntü bilgilerini al

width \= struct.unpack('<i', dib\_header\[4:8\])\[0\]

height \= struct.unpack('<i', dib\_header\[8:12\])\[0\]

bit\_depth \= struct.unpack('<H', dib\_header\[14:16\])\[0\]

return width, height, data\_offset, bit\_depth

def analyze\_bmp\_colors(self, file\_path):

"""

BMP dosyasındaki benzersiz renkleri ve piksel sayısını analiz eder.

Args:

file\_path (str): BMP dosyasının yolu

Returns:

tuple: (toplam piksel sayısı, benzersiz renk sayısı, renk dağılımı)

"""

try:

\# BMP header bilgilerini oku

width, height, data\_offset, bit\_depth \= self.read\_bmp\_header(file\_path)

\# Görüntünün toplam piksel sayısı

total\_pixels \= width \* abs(height) \# Height negatif olabilir (görüntü yönü)

\# Renk analizi için gerekli ayarlar

bytes\_per\_pixel \= bit\_depth // 8

row\_size \= ((width \* bit\_depth + 31) // 32) \* 4 \# Her satır 4 byte'a göre hizalanır

\# Benzersiz renkleri saymak için counter kullan

unique\_colors \= Counter()

with open(file\_path, 'rb') as f:

\# Piksel verisinin başlangıcına git

f.seek(data\_offset)

for y in range(abs(height)):

row\_start \= f.tell()

for x in range(width):

\# Piksel verisini oku

pixel\_data \= f.read(bytes\_per\_pixel)

if len(pixel\_data) < bytes\_per\_pixel:

print(f"Uyarı: Dosya beklenenden kısa ({y}/{height} satırında kesildi)")

break

\# Renk değeri olarak tüm pikseli bir tuple olarak kaydet

\# BGR formatı için: (Blue, Green, Red)

if bit\_depth \== 24: \# 24-bit

blue, green, red \= pixel\_data

color \= (red, green, blue) \# RGB formatında kaydet

elif bit\_depth \== 32: \# 32-bit (BGRA)

blue, green, red, alpha \= pixel\_data

color \= (red, green, blue, alpha)

elif bit\_depth \== 8: \# 8-bit (indeksli)

color \= pixel\_data\[0\]

else:

\# Diğer bit derinlikleri için basit bir yaklaşım

color \= pixel\_data

\# Rengi say

unique\_colors\[color\] += 1

\# Satır sonuna git (padding var ise atla)

f.seek(row\_start + row\_size)

\# Analiz sonuçları

unique\_color\_count \= len(unique\_colors)

return total\_pixels, unique\_color\_count, unique\_colors

except Exception as e:

messagebox.showerror("Analiz Hatası", f"BMP analizi sırasında bir hata oluştu:\\n{str(e)}")

return None

def main():

root \= tk.Tk()

app \= BMPAnalyzerApp(root)

root.mainloop()

if \_\_name\_\_ \== "\_\_main\_\_":

main()