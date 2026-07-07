# Source: https://github.com/soyluoltu/bmp-manipulations/blob/main/source/bmp-manipulator.py

[soyluoltu](https://github.com/soyluoltu) / **[bmp-manipulations](https://github.com/soyluoltu/bmp-manipulations)** Public

- [Notifications](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations)
- [Star 0](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations)
 

 

## FilesExpand file tree

 main

/

# bmp-manipulator.py

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

## History

[History](https://github.com/soyluoltu/bmp-manipulations/commits/main/source/bmp-manipulator.py)

History

994 lines (775 loc) · 38.2 KB

 main

/

# bmp-manipulator.py

Copy path

Top

## File metadata and controls

- Code
 
- Blame
 

994 lines (775 loc) · 38.2 KB

[Raw](https://github.com/soyluoltu/bmp-manipulations/raw/refs/heads/main/source/bmp-manipulator.py)

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

558

559

560

561

562

563

564

565

566

567

568

569

570

571

572

573

574

575

576

577

578

579

580

581

582

583

584

585

586

587

588

589

590

591

592

593

594

595

596

597

598

599

600

601

602

603

604

605

606

607

608

609

610

611

612

613

614

615

616

617

618

619

620

621

622

623

624

625

626

627

628

629

630

631

632

633

634

635

636

637

638

639

640

641

642

643

644

645

646

647

648

649

650

651

652

653

654

655

656

657

658

659

660

661

662

663

664

665

666

667

668

669

670

671

672

673

674

675

676

677

678

679

680

681

682

683

684

685

686

687

688

689

690

691

692

693

694

695

696

697

698

699

700

701

702

703

704

705

706

707

708

709

710

711

712

713

714

715

716

717

718

719

720

721

722

723

724

725

726

727

728

729

730

731

732

733

734

735

736

737

738

739

740

741

742

743

744

745

746

747

748

749

750

751

752

753

754

755

756

757

758

759

760

761

762

763

764

765

766

767

768

769

770

771

772

773

774

775

776

777

778

779

780

781

782

783

784

785

786

787

788

789

790

791

792

793

794

795

796

797

798

799

800

801

802

803

804

805

806

807

808

809

810

811

812

813

814

815

816

817

818

819

820

821

822

823

824

825

826

827

828

829

830

831

832

833

834

835

836

837

838

839

840

841

842

843

844

845

846

847

848

849

850

851

852

853

854

855

856

857

858

859

860

861

862

863

864

865

866

867

868

869

870

871

872

873

874

875

876

877

878

879

880

881

882

883

884

885

886

887

888

889

890

891

892

893

894

895

896

897

898

899

900

901

902

903

904

905

906

907

908

909

910

911

912

913

914

915

916

917

918

919

920

921

922

923

924

925

926

927

928

929

930

931

932

933

934

935

936

937

938

939

940

941

942

943

944

945

946

947

948

949

950

951

952

953

954

955

956

957

958

959

960

961

962

963

964

965

966

967

968

969

970

971

972

973

974

975

976

977

978

979

980

981

982

983

984

985

986

987

988

989

990

991

992

993

994

#!/usr/bin/env python3

\# -\*- coding: utf-8 -\*-

"""

BMP Manipülatörü

\===============

BMP dosyalarında metadata işleme ve steganografi araçları.

Bu modül BMP dosyalarında başlık manipülasyonu, metadata ekleme/çıkarma

ve steganografi teknikleri için kapsamlı araçlar sunar.

"""

import os

import sys

import struct

import argparse

import json

import hashlib

import binascii

import base64

from datetime import datetime

from typing import Dict, List, Tuple, Union, Optional, BinaryIO, Any

from dataclasses import dataclass

from enum import Enum, auto

try:

from cryptography.hazmat.primitives.ciphers.aead import AESGCM, ChaCha20Poly1305

from cryptography.hazmat.primitives import hashes

from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC

CRYPTO\_AVAILABLE \= True

except ImportError:

CRYPTO\_AVAILABLE \= False

\_\_version\_\_ \= "1.0.0"

\_\_author\_\_ \= "BMP Manipülatör Ekibi"

\# BMP Dosya Başlığı Yapısı

BMP\_HEADER\_FORMAT \= '<2sIHHI'

BMP\_HEADER\_SIZE \= 14

\# DIB Başlık Tipleri

DIB\_HEADER\_SIZES \= {

12: 'BITMAPCOREHEADER',

40: 'BITMAPINFOHEADER',

52: 'BITMAPV2INFOHEADER',

56: 'BITMAPV3INFOHEADER',

108: 'BITMAPV4HEADER',

124: 'BITMAPV5HEADER'

}

\# Sıkıştırma Metodları

BI\_COMPRESSION \= {

0: 'BI\_RGB', \# Sıkıştırma yok

1: 'BI\_RLE8', \# 8-bit RLE sıkıştırma

2: 'BI\_RLE4', \# 4-bit RLE sıkıştırma

3: 'BI\_BITFIELDS', \# Bit alanları

4: 'BI\_JPEG', \# JPEG sıkıştırma

5: 'BI\_PNG', \# PNG sıkıştırma

6: 'BI\_ALPHABITFIELDS', \# Alfa bit alanları

11: 'BI\_CMYK', \# CMYK renk uzayı

12: 'BI\_CMYKRLE8', \# CMYK 8-bit RLE

13: 'BI\_CMYKRLE4' \# CMYK 4-bit RLE

}

\# Metadata Blok İmzası

METADATA\_SIGNATURE \= b'BMPM'

\# LSB Steganografi için varsayılan ayarlar

DEFAULT\_LSB\_DEPTH \= 1 \# Değiştirilecek bit sayısı

DEFAULT\_LSB\_CHANNELS \= 3 \# Tüm renkler (R, G, B)

class BMPError(Exception):

"""BMP işleme ile ilgili hatalar için özel istisna."""

pass

class MetadataError(Exception):

"""Metadata işleme ile ilgili hatalar için özel istisna."""

pass

class SteganographyError(Exception):

"""Steganografi işleme ile ilgili hatalar için özel istisna."""

pass

class MetadataStorageMethod(Enum):

"""Metadata depolama yöntemleri."""

HEADER\_EXTENSION \= auto()

APPLICATION\_BLOCK \= auto()

EOF\_APPEND \= auto()

class StegoMethod(Enum):

"""Desteklenen steganografi yöntemleri."""

LSB \= auto()

PALETTE \= auto()

HEADER \= auto()

EOF \= auto()

@dataclass

class BMPFileHeader:

"""BMP dosya başlığı yapısı."""

signature: bytes \# 'BM'

file\_size: int \# Bayt olarak dosya boyutu

reserved1: int \# Rezerve edilmiş (genellikle 0)

reserved2: int \# Rezerve edilmiş (genellikle 0)

pixel\_offset: int \# Piksel verilerine offset

@dataclass

class DIBHeader:

"""DIB başlığı için temel sınıf."""

header\_size: int \# Başlık boyutu

width: int \# Piksel olarak genişlik

height: int \# Piksel olarak yükseklik

planes: int \# Renk düzlemleri (her zaman 1)

bit\_count: int \# Piksel başına bit

compression: int \# Sıkıştırma yöntemi

image\_size: int \# Görüntü verisi boyutu

x\_ppm: int \# X çözünürlüğü (piksel/metre)

y\_ppm: int \# Y çözünürlüğü (piksel/metre)

colors\_used: int \# Kullanılan renk sayısı

colors\_important: int \# Önemli renk sayısı

raw\_data: bytes \# Tüm ham başlık verisi

class Metadata:

"""BMP dosyasına eklenecek metadata yöneticisi."""

def \_\_init\_\_(self):

self.entries \= {}

self.creation\_date \= datetime.now().isoformat()

def add(self, key: str, value: Union\[str, bytes, int, float, dict, list\]) \-> None:

"""Yeni bir metadata girişi ekler."""

if isinstance(value, (dict, list)):

value \= json.dumps(value, ensure\_ascii\=False)

if not isinstance(value, (str, bytes, int, float)):

raise MetadataError(f"Desteklenmeyen metadata değer tipi: {type(value)}")

self.entries\[key\] \= value

def get(self, key: str) \-> Any:

"""Belirtilen anahtara sahip metadata değerini döndürür."""

if key not in self.entries:

raise MetadataError(f"Metadata anahtarı bulunamadı: {key}")

return self.entries\[key\]

def remove(self, key: str) \-> None:

"""Belirtilen anahtara sahip metadata girişini siler."""

if key not in self.entries:

raise MetadataError(f"Metadata anahtarı bulunamadı: {key}")

del self.entries\[key\]

def to\_dict(self) \-> Dict:

"""Metadata'yı sözlük olarak döndürür."""

return {

"creation\_date": self.creation\_date,

"entries": self.entries

}

def to\_bytes(self) \-> bytes:

"""Metadata'yı ikili formata dönüştürür."""

result \= bytearray()

\# Giriş sayısını ekle (2 bayt)

result.extend(struct.pack("<H", len(self.entries)))

\# Her girişi ekle

for key, value in self.entries.items():

key\_bytes \= key.encode('utf-8')

\# Değeri uygun formata dönüştür

if isinstance(value, str):

value\_bytes \= value.encode('utf-8')

elif isinstance(value, bytes):

value\_bytes \= value

elif isinstance(value, (int, float)):

value\_bytes \= str(value).encode('utf-8')

else:

raise MetadataError(f"Desteklenmeyen metadata değer tipi: {type(value)}")

\# Anahtar uzunluğu (2 bayt)

result.extend(struct.pack("<H", len(key\_bytes)))

\# Değer uzunluğu (4 bayt)

result.extend(struct.pack("<I", len(value\_bytes)))

\# Anahtar

result.extend(key\_bytes)

\# Değer

result.extend(value\_bytes)

return bytes(result)

@classmethod

def from\_bytes(cls, data: bytes) \-> 'Metadata':

"""İkili veriden metadata oluşturur."""

metadata \= cls()

\# Girdi sayısını oku

if len(data) < 2:

raise MetadataError("Geçersiz metadata format: veri çok kısa")

entry\_count \= struct.unpack("<H", data\[:2\])\[0\]

position \= 2

for \_ in range(entry\_count):

if position + 6 \> len(data):

raise MetadataError("Geçersiz metadata format: beklenmeyen dosya sonu")

\# Anahtar ve değer uzunluklarını oku

key\_length \= struct.unpack("<H", data\[position:position+2\])\[0\]

position += 2

value\_length \= struct.unpack("<I", data\[position:position+4\])\[0\]

position += 4

if position + key\_length + value\_length \> len(data):

raise MetadataError("Geçersiz metadata format: eksik veri")

\# Anahtar ve değeri oku

key \= data\[position:position+key\_length\].decode('utf-8')

position += key\_length

value \= data\[position:position+value\_length\]

position += value\_length

\# UTF-8 metin olarak değeri çözmeyi dene

try:

value \= value.decode('utf-8')

\# JSON olarak çözmeyi dene

try:

value \= json.loads(value)

except json.JSONDecodeError:

pass \# Normal metin olarak bırak

except UnicodeDecodeError:

pass \# İkili veri olarak bırak

metadata.add(key, value)

return metadata

class BMPFile:

"""BMP dosyası yükleme, işleme ve kaydetme işlemleri için ana sınıf."""

def \_\_init\_\_(self, file\_path: Optional\[str\] \= None):

self.file\_path \= file\_path

self.file\_header: Optional\[BMPFileHeader\] \= None

self.dib\_header: Optional\[DIBHeader\] \= None

self.pixel\_data: Optional\[bytes\] \= None

self.palette: Optional\[bytes\] \= None

self.metadata: Optional\[Metadata\] \= None

self.raw\_data: Optional\[bytes\] \= None

if file\_path:

self.load(file\_path)

def load(self, file\_path: str) \-> None:

"""BMP dosyasını yükler ve analiz eder."""

self.file\_path \= file\_path

with open(file\_path, 'rb') as f:

self.raw\_data \= f.read()

if len(self.raw\_data) < BMP\_HEADER\_SIZE:

raise BMPError("Geçersiz BMP dosyası: dosya çok küçük")

\# Dosya başlığını ayrıştır

signature, file\_size, reserved1, reserved2, pixel\_offset \= struct.unpack(

BMP\_HEADER\_FORMAT, self.raw\_data\[:BMP\_HEADER\_SIZE\]

)

if signature != b'BM':

raise BMPError(f"Geçersiz BMP imzası: {signature}")

self.file\_header \= BMPFileHeader(

signature\=signature,

file\_size\=file\_size,

reserved1\=reserved1,

reserved2\=reserved2,

pixel\_offset\=pixel\_offset

)

\# DIB başlık boyutunu oku

if len(self.raw\_data) < BMP\_HEADER\_SIZE + 4:

raise BMPError("Geçersiz BMP dosyası: DIB başlığı eksik")

dib\_size \= struct.unpack('<I', self.raw\_data\[BMP\_HEADER\_SIZE:BMP\_HEADER\_SIZE+4\])\[0\]

if dib\_size not in DIB\_HEADER\_SIZES:

raise BMPError(f"Tanınmayan DIB başlık boyutu: {dib\_size}")

if len(self.raw\_data) < BMP\_HEADER\_SIZE + dib\_size:

raise BMPError("Geçersiz BMP dosyası: DIB başlığı eksik")

\# Minimum BITMAPINFOHEADER formatı

if dib\_size \>= 40:

header\_format \= '<IiiHHIIiiII'

dib\_data \= self.raw\_data\[BMP\_HEADER\_SIZE:BMP\_HEADER\_SIZE+40\]

try:

(header\_size, width, height, planes, bit\_count,

compression, image\_size, x\_ppm, y\_ppm,

colors\_used, colors\_important) \= struct.unpack(header\_format, dib\_data)

except struct.error as e:

raise BMPError(f"DIB başlığı ayrıştırma hatası: {e}")

self.dib\_header \= DIBHeader(

header\_size\=header\_size,

width\=width,

height\=abs(height), \# Yükseklik negatif olabilir - alt-üst olmayan görüntü

planes\=planes,

bit\_count\=bit\_count,

compression\=compression,

image\_size\=image\_size,

x\_ppm\=x\_ppm,

y\_ppm\=y\_ppm,

colors\_used\=colors\_used,

colors\_important\=colors\_important,

raw\_data\=self.raw\_data\[BMP\_HEADER\_SIZE:BMP\_HEADER\_SIZE+dib\_size\]

)

else:

\# BITMAPCOREHEADER gibi eski formatları desteklemiyoruz

raise BMPError(f"Desteklenmeyen BMP formatı: {DIB\_HEADER\_SIZES.get(dib\_size, 'Bilinmeyen')}")

\# Renk paletini yükle

if self.dib\_header.bit\_count <= 8:

palette\_start \= BMP\_HEADER\_SIZE + dib\_size

palette\_size \= self.file\_header.pixel\_offset \- palette\_start

if palette\_size \> 0:

self.palette \= self.raw\_data\[palette\_start:palette\_start+palette\_size\]

\# Piksel verisini yükle

pixel\_start \= self.file\_header.pixel\_offset

if pixel\_start \> len(self.raw\_data):

raise BMPError("Geçersiz piksel verisi offseti")

self.pixel\_data \= self.raw\_data\[pixel\_start:file\_size\]

\# Dosya sonunda metadata olup olmadığını kontrol et

self.\_extract\_metadata()

def \_extract\_metadata(self) \-> None:

"""Dosyadan metadata çıkarmayı dener."""

if not self.raw\_data or len(self.raw\_data) <= self.file\_header.file\_size:

return \# Metadata yok

\# Dosya sonunda BMPM imzasını ara

eof\_data \= self.raw\_data\[self.file\_header.file\_size:\]

if len(eof\_data) \>= 16 and eof\_data\[:4\] \== METADATA\_SIGNATURE:

try:

\# Metadata blok boyutunu oku

block\_size \= struct.unpack("<I", eof\_data\[4:8\])\[0\]

if block\_size <= len(eof\_data):

\# Sürüm, şifreleme bayrakları vb. oku

version \= struct.unpack("<H", eof\_data\[8:10\])\[0\]

is\_encrypted \= eof\_data\[10\] \== 1

\# Şifreli metadata henüz desteklenmiyor

if is\_encrypted and not CRYPTO\_AVAILABLE:

print("UYARI: Şifreli metadata bulundu ama şifreleme kütüphanesi yüklü değil")

return

\# Metadata verilerini çıkar

metadata\_data \= eof\_data\[12:\-4\] \# Son 4 bayt sağlama toplamıdır

\# Sağlama toplamını doğrula

stored\_checksum \= struct.unpack("<I", eof\_data\[block\_size\-4:block\_size\])\[0\]

calculated\_checksum \= binascii.crc32(eof\_data\[:block\_size\-4\]) & 0xFFFFFFFF

if stored\_checksum != calculated\_checksum:

print("UYARI: Metadata sağlama toplamı eşleşmiyor, veri bozulmuş olabilir")

\# Şifreli veriyi çöz (burada uygulanmamış)

if is\_encrypted:

\# Bu şifreleme/şifre çözme için yer tutucudur

pass

\# Metadata'yı yükle

self.metadata \= Metadata.from\_bytes(metadata\_data)

except Exception as e:

print(f"Metadata ayrıştırma hatası: {e}")

def add\_metadata(self, metadata: Metadata, method: MetadataStorageMethod \= MetadataStorageMethod.EOF\_APPEND,

password: Optional\[str\] \= None) \-> None:

"""BMP dosyasına metadata ekler."""

self.metadata \= metadata

\# Şimdilik sadece EOF\_APPEND metodu destekleniyor

if method != MetadataStorageMethod.EOF\_APPEND:

raise NotImplementedError(f"Henüz desteklenmeyen metadata depolama metodu: {method}")

def save(self, output\_path: str) \-> None:

"""BMP dosyasını kaydeder, metadata dahil."""

if not self.file\_header or not self.dib\_header or not self.pixel\_data:

raise BMPError("Kaydetmeden önce geçerli bir BMP yüklenmelidir")

\# Orijinal BMP verisi

output\_data \= bytearray(self.raw\_data\[:self.file\_header.file\_size\])

\# Metadata ekle (varsa)

if self.metadata:

metadata\_bytes \= self.\_prepare\_metadata\_block()

output\_data.extend(metadata\_bytes)

with open(output\_path, 'wb') as f:

f.write(output\_data)

def \_prepare\_metadata\_block(self, password: Optional\[str\] \= None) \-> bytes:

"""Metadata bloğunu hazırlar."""

if not self.metadata:

return b''

\# Metadata verilerini hazırla

metadata\_data \= self.metadata.to\_bytes()

is\_encrypted \= False

\# Şifreleme (eğer parola sağlanmışsa ve kriptografi kütüphanesi mevcutsa)

if password and CRYPTO\_AVAILABLE:

metadata\_data \= self.\_encrypt\_data(metadata\_data, password)

is\_encrypted \= True

\# Metadata başlığını oluştur

header \= bytearray()

header.extend(METADATA\_SIGNATURE) \# 4 bayt imza

\# Tüm blok boyutunu hesaplamak için geçici yer tutucu

header.extend(b'\\x00\\x00\\x00\\x00') \# 4 bayt blok boyutu (şimdilik 0)

\# Sürüm, bayraklar vb.

header.extend(struct.pack("<H", 1)) \# 2 bayt sürüm numarası

header.append(1 if is\_encrypted else 0) \# 1 bayt şifreleme bayrağı

header.append(0) \# 1 bayt rezerve

\# Metadata verilerini ekle

full\_block \= bytearray(header)

full\_block.extend(metadata\_data)

\# Sağlama toplamı hesapla ve ekle

checksum \= binascii.crc32(full\_block) & 0xFFFFFFFF

full\_block.extend(struct.pack("<I", checksum))

\# Gerçek blok boyutunu güncelle

block\_size \= len(full\_block)

full\_block\[4:8\] \= struct.pack("<I", block\_size)

return bytes(full\_block)

def \_encrypt\_data(self, data: bytes, password: str) \-> bytes:

"""Veriyi şifreler."""

if not CRYPTO\_AVAILABLE:

raise RuntimeError("Şifreleme için cryptography kütüphanesi gereklidir")

\# 16 baytlık rastgele tuz oluştur

salt \= os.urandom(16)

\# Anahtar türetme

kdf \= PBKDF2HMAC(

algorithm\=hashes.SHA256(),

length\=32,

salt\=salt,

iterations\=100000,

)

key \= kdf.derive(password.encode())

\# AES-GCM ile şifreleme

aesgcm \= AESGCM(key)

nonce \= os.urandom(12)

ciphertext \= aesgcm.encrypt(nonce, data, None)

\# Salt + nonce + şifreli metni birleştir

return salt + nonce + ciphertext

def \_decrypt\_data(self, encrypted\_data: bytes, password: str) \-> bytes:

"""Şifreli veriyi çözer."""

if not CRYPTO\_AVAILABLE:

raise RuntimeError("Şifre çözme için cryptography kütüphanesi gereklidir")

\# Tuz ve nonce'yi ayır

salt \= encrypted\_data\[:16\]

nonce \= encrypted\_data\[16:28\]

ciphertext \= encrypted\_data\[28:\]

\# Anahtarı türet

kdf \= PBKDF2HMAC(

algorithm\=hashes.SHA256(),

length\=32,

salt\=salt,

iterations\=100000,

)

key \= kdf.derive(password.encode())

\# AES-GCM ile şifre çözme

aesgcm \= AESGCM(key)

try:

return aesgcm.decrypt(nonce, ciphertext, None)

except Exception:

raise MetadataError("Şifre çözme başarısız: yanlış parola veya bozuk veri")

@property

def width(self) \-> int:

"""Görüntü genişliğini döndürür."""

return self.dib\_header.width if self.dib\_header else 0

@property

def height(self) \-> int:

"""Görüntü yüksekliğini döndürür."""

return self.dib\_header.height if self.dib\_header else 0

@property

def bits\_per\_pixel(self) \-> int:

"""Piksel başına bit sayısını döndürür."""

return self.dib\_header.bit\_count if self.dib\_header else 0

@property

def file\_size(self) \-> int:

"""Dosya boyutunu döndürür."""

return self.file\_header.file\_size if self.file\_header else 0

@property

def pixel\_data\_offset(self) \-> int:

"""Piksel verilerine offseti döndürür."""

return self.file\_header.pixel\_offset if self.file\_header else 0

@property

def compression\_type(self) \-> str:

"""Sıkıştırma türünü döndürür."""

if not self.dib\_header:

return "Bilinmeyen"

return BI\_COMPRESSION.get(self.dib\_header.compression, f"Bilinmeyen ({self.dib\_header.compression})")

@property

def header\_type(self) \-> str:

"""Başlık tipini döndürür."""

if not self.dib\_header:

return "Bilinmeyen"

return DIB\_HEADER\_SIZES.get(self.dib\_header.header\_size, f"Bilinmeyen ({self.dib\_header.header\_size})")

def get\_info(self) \-> Dict\[str, Any\]:

"""BMP dosyası hakkında bilgileri sözlük olarak döndürür."""

if not self.file\_header or not self.dib\_header:

return {"error": "BMP yüklenmedi"}

info \= {

"file\_name": os.path.basename(self.file\_path) if self.file\_path else "Bilinmeyen",

"file\_size": self.file\_size,

"header\_type": self.header\_type,

"dimensions": f"{self.width}x{self.height}",

"bit\_depth": self.bits\_per\_pixel,

"compression": self.compression\_type,

"has\_metadata": self.metadata is not None

}

if self.metadata:

info\["metadata\_keys"\] \= list(self.metadata.entries.keys())

return info

def extract\_metadata(self, password: Optional\[str\] \= None) \-> Optional\[Metadata\]:

"""Dosyadan metadata çıkarır ve döndürür."""

return self.metadata

class LSBSteganography:

"""En Az Önemli Bit (LSB) steganografi sınıfı."""

def \_\_init\_\_(self, bmp\_file: BMPFile):

self.bmp\_file \= bmp\_file

if not self.bmp\_file.pixel\_data:

raise SteganographyError("Steganografi için piksel verisi gereklidir")

\# BMP formatı kontrolü

if self.bmp\_file.bits\_per\_pixel != 24 and self.bmp\_file.bits\_per\_pixel != 32:

raise SteganographyError(f"LSB steganografi sadece 24-bit veya 32-bit BMP dosyalarını destekler, şu anki: {self.bmp\_file.bits\_per\_pixel}")

if self.bmp\_file.compression\_type != "BI\_RGB":

raise SteganographyError(f"LSB steganografi sıkıştırmasız BMP dosyaları gerektirir, şu anki: {self.bmp\_file.compression\_type}")

def calculate\_capacity(self, bit\_depth: int \= DEFAULT\_LSB\_DEPTH, channels: int \= DEFAULT\_LSB\_CHANNELS) \-> int:

"""LSB steganografi ile saklanabilecek maksimum bayt sayısını hesaplar."""

bytes\_per\_pixel \= self.bmp\_file.bits\_per\_pixel // 8

usable\_channels \= min(channels, bytes\_per\_pixel)

\# Toplam bit kapasitesi

total\_bits \= self.bmp\_file.width \* self.bmp\_file.height \* usable\_channels \* bit\_depth

\# Kapasite baytlara dönüştürülür (8 bit = 1 bayt)

byte\_capacity \= total\_bits // 8

\# 4 baytlık veri uzunluğu bilgisi için alan ayır

return max(0, byte\_capacity \- 4)

def hide\_data(self, data: bytes, bit\_depth: int \= DEFAULT\_LSB\_DEPTH,

channels: int \= DEFAULT\_LSB\_CHANNELS, password: Optional\[str\] \= None) \-> None:

"""Verilen veriyi BMP görüntüsünde gizler."""

max\_capacity \= self.calculate\_capacity(bit\_depth, channels)

if len(data) \> max\_capacity:

raise SteganographyError(f"Veri çok büyük: {len(data)} bayt > {max\_capacity} bayt (maksimum kapasite)")

\# Şifreleme (eğer parola sağlanmışsa)

if password and CRYPTO\_AVAILABLE:

data \= self.\_encrypt\_data(data, password)

\# 4 baytlık veri uzunluğu + veri

data\_to\_hide \= struct.pack("<I", len(data)) + data

\# Veriyi bit dizisine dönüştür

data\_bits \= \[\]

for byte in data\_to\_hide:

for i in range(8):

data\_bits.append((byte \>> i) & 1)

\# BMP piksel verisinin bir kopyasını oluştur

new\_pixel\_data \= bytearray(self.bmp\_file.pixel\_data)

bytes\_per\_pixel \= self.bmp\_file.bits\_per\_pixel // 8

usable\_channels \= min(channels, bytes\_per\_pixel)

bit\_index \= 0

total\_bits \= len(data\_bits)

\# Her pikseldeki her kanalın her bitini güncelle

for i in range(0, len(new\_pixel\_data), bytes\_per\_pixel):

\# Her kanal için

for c in range(usable\_channels):

if i + c \>= len(new\_pixel\_data):

break

\# Her bit için (belirtilen bit derinliğine göre)

for b in range(bit\_depth):

if bit\_index \>= total\_bits:

break

\# En düşük önemli bit(ler)i değiştir

if data\_bits\[bit\_index\] \== 1:

new\_pixel\_data\[i + c\] |= (1 << b)

else:

new\_pixel\_data\[i + c\] &= ~(1 << b)

bit\_index += 1

if bit\_index \>= total\_bits:

break

if bit\_index \>= total\_bits:

break

\# Güncellenmiş piksel verisini BMP dosyasına kaydet

self.bmp\_file.pixel\_data \= bytes(new\_pixel\_data)

def extract\_data(self, bit\_depth: int \= DEFAULT\_LSB\_DEPTH,

channels: int \= DEFAULT\_LSB\_CHANNELS,

password: Optional\[str\] \= None) \-> bytes:

"""BMP görüntüsündeki gizli veriyi çıkarır."""

if not self.bmp\_file.pixel\_data:

raise SteganographyError("Veri çıkarmak için piksel verisi gereklidir")

bytes\_per\_pixel \= self.bmp\_file.bits\_per\_pixel // 8

usable\_channels \= min(channels, bytes\_per\_pixel)

\# Önce 4 baytlık veri uzunluğunu çıkar

length\_bits \= \[\]

bit\_index \= 0

\# Piksel verilerinden bitleri çıkar

for i in range(0, len(self.bmp\_file.pixel\_data), bytes\_per\_pixel):

for c in range(usable\_channels):

if i + c \>= len(self.bmp\_file.pixel\_data):

break

for b in range(bit\_depth):

bit\_value \= (self.bmp\_file.pixel\_data\[i + c\] \>> b) & 1

length\_bits.append(bit\_value)

bit\_index += 1

if bit\_index \>= 32: \# 4 bayt = 32 bit

break

if bit\_index \>= 32:

break

if bit\_index \>= 32:

break

\# Bit dizisini 4 baytlık uzunluk değerine dönüştür

length\_bytes \= bytearray(4)

for i, bit in enumerate(length\_bits):

byte\_index \= i // 8

bit\_position \= i % 8

if bit:

length\_bytes\[byte\_index\] |= (1 << bit\_position)

data\_length \= struct.unpack("<I", length\_bytes)\[0\]

\# Maksimum kapasiteyi kontrol et

max\_capacity \= self.calculate\_capacity(bit\_depth, channels)

if data\_length \> max\_capacity:

raise SteganographyError(f"Geçersiz veri uzunluğu: {data\_length} > {max\_capacity}")

\# Şimdi gerçek veriyi çıkar

data\_bits \= \[\]

bit\_index \= 32 \# Veri uzunluğundan sonra başla

bits\_needed \= data\_length \* 8

for i in range(0, len(self.bmp\_file.pixel\_data), bytes\_per\_pixel):

for c in range(usable\_channels):

if i + c \>= len(self.bmp\_file.pixel\_data):

break

for b in range(bit\_depth):

if bit\_index \>= 32 + bits\_needed:

break

bit\_value \= (self.bmp\_file.pixel\_data\[i + c\] \>> b) & 1

data\_bits.append(bit\_value)

bit\_index += 1

if bit\_index \>= 32 + bits\_needed:

break

if bit\_index \>= 32 + bits\_needed:

break

\# Bit dizisini baytlara dönüştür

extracted\_data \= bytearray(data\_length)

for i, bit in enumerate(data\_bits\[:bits\_needed\]):

byte\_index \= i // 8

bit\_position \= i % 8

if bit:

extracted\_data\[byte\_index\] |= (1 << bit\_position)

\# Şifre çözme (eğer parola sağlanmışsa)

if password and CRYPTO\_AVAILABLE:

try:

extracted\_data \= self.\_decrypt\_data(extracted\_data, password)

except Exception as e:

raise SteganographyError(f"Şifre çözme hatası: {e}")

return bytes(extracted\_data)

def \_encrypt\_data(self, data: bytes, password: str) \-> bytes:

"""Veriyi şifreler."""

if not CRYPTO\_AVAILABLE:

raise RuntimeError("Şifreleme için cryptography kütüphanesi gereklidir")

\# 16 baytlık rastgele tuz oluştur

salt \= os.urandom(16)

\# Anahtar türetme

kdf \= PBKDF2HMAC(

algorithm\=hashes.SHA256(),

length\=32,

salt\=salt,

iterations\=100000,

)

key \= kdf.derive(password.encode())

\# AES-GCM ile şifreleme

aesgcm \= AESGCM(key)

nonce \= os.urandom(12)

ciphertext \= aesgcm.encrypt(nonce, data, None)

\# Salt + nonce + şifreli metni birleştir

return salt + nonce + ciphertext

def \_decrypt\_data(self, encrypted\_data: bytes, password: str) \-> bytes:

"""Şifreli veriyi çözer."""

if not CRYPTO\_AVAILABLE:

raise RuntimeError("Şifre çözme için cryptography kütüphanesi gereklidir")

if len(encrypted\_data) < 28: \# 16 (salt) + 12 (nonce) bayt minimum

raise SteganographyError("Geçersiz şifreli veri: çok kısa")

\# Tuz ve nonce'yi ayır

salt \= encrypted\_data\[:16\]

nonce \= encrypted\_data\[16:28\]

ciphertext \= encrypted\_data\[28:\]

\# Anahtarı türet

kdf \= PBKDF2HMAC(

algorithm\=hashes.SHA256(),

length\=32,

salt\=salt,

iterations\=100000,

)

key \= kdf.derive(password.encode())

\# AES-GCM ile şifre çözme

aesgcm \= AESGCM(key)

try:

return aesgcm.decrypt(nonce, ciphertext, None)

except Exception:

raise SteganographyError("Şifre çözme başarısız: yanlış parola veya bozuk veri")

def hide\_text(self, text: str, encoding: str \= 'utf-8', \*\*kwargs) \-> None:

"""Metin mesajını BMP görüntüsünde gizler."""

text\_data \= text.encode(encoding)

self.hide\_data(text\_data, \*\*kwargs)

@staticmethod

def extract\_text(bmp\_path: str, encoding: str \= 'utf-8', \*\*kwargs) \-> str:

"""BMP görüntüsündeki gizli metni çıkarır."""

bmp \= BMPFile(bmp\_path)

stego \= LSBSteganography(bmp)

extracted\_data \= stego.extract\_data(\*\*kwargs)

try:

return extracted\_data.decode(encoding)

except UnicodeDecodeError:

raise SteganographyError(f"{encoding} olarak metin çözme başarısız, farklı bir kodlama deneyin veya parola gerekebilir")

def main():

"""BMP Manipülatörü için ana komut satırı arayüzü."""

parser \= argparse.ArgumentParser(description\="BMP Manipülatörü - BMP dosyalarını değiştirme ve steganografi aracı")

subparsers \= parser.add\_subparsers(dest\="command", help\="Komut")

\# info komutu

parser\_info \= subparsers.add\_parser("info", help\="BMP dosyası hakkında bilgi göster")

parser\_info.add\_argument("file", help\="BMP dosya yolu")

\# metadata komutları

parser\_metadata \= subparsers.add\_parser("metadata", help\="Metadata işlemleri")

metadata\_subparsers \= parser\_metadata.add\_subparsers(dest\="metadata\_command", help\="Metadata komutu")

\# metadata add komutu

parser\_metadata\_add \= metadata\_subparsers.add\_parser("add", help\="BMP dosyasına metadata ekle")

parser\_metadata\_add.add\_argument("file", help\="BMP dosya yolu")

parser\_metadata\_add.add\_argument("--key", required\=True, help\="Metadata anahtarı")

parser\_metadata\_add.add\_argument("--value", required\=True, help\="Metadata değeri")

parser\_metadata\_add.add\_argument("--output", help\="Çıktı dosya yolu (belirtilmezse orijinal dosya üzerine yazılır)")

parser\_metadata\_add.add\_argument("--password", help\="Metadata şifreleme parolası")

\# metadata extract komutu

parser\_metadata\_extract \= metadata\_subparsers.add\_parser("extract", help\="BMP dosyasından metadata çıkar")

parser\_metadata\_extract.add\_argument("file", help\="BMP dosya yolu")

parser\_metadata\_extract.add\_argument("--password", help\="Metadata şifre çözme parolası")

\# stego komutları

parser\_stego \= subparsers.add\_parser("stego", help\="Steganografi işlemleri")

stego\_subparsers \= parser\_stego.add\_subparsers(dest\="stego\_command", help\="Steganografi komutu")

\# stego hide komutu

parser\_stego\_hide \= stego\_subparsers.add\_parser("hide", help\="BMP dosyasında veri gizle")

parser\_stego\_hide.add\_argument("file", help\="Taşıyıcı BMP dosya yolu")

group \= parser\_stego\_hide.add\_mutually\_exclusive\_group(required\=True)

group.add\_argument("--text", help\="Gizlenecek metin")

group.add\_argument("--file", help\="Gizlenecek dosya yolu", dest\="hide\_file")

parser\_stego\_hide.add\_argument("--output", required\=True, help\="Çıktı BMP dosya yolu")

parser\_stego\_hide.add\_argument("--bit-depth", type\=int, default\=DEFAULT\_LSB\_DEPTH, help\=f"Bit derinliği (varsayılan: {DEFAULT\_LSB\_DEPTH})")

parser\_stego\_hide.add\_argument("--channels", type\=int, default\=DEFAULT\_LSB\_CHANNELS, help\=f"Kullanılacak renk kanalı sayısı (varsayılan: {DEFAULT\_LSB\_CHANNELS})")

parser\_stego\_hide.add\_argument("--password", help\="Veri şifreleme parolası")

\# stego extract komutu

parser\_stego\_extract \= stego\_subparsers.add\_parser("extract", help\="BMP dosyasından gizli veri çıkar")

parser\_stego\_extract.add\_argument("file", help\="BMP dosya yolu")

parser\_stego\_extract.add\_argument("--output", help\="Çıktı dosya yolu (belirtilmezse metin olarak göster)")

parser\_stego\_extract.add\_argument("--bit-depth", type\=int, default\=DEFAULT\_LSB\_DEPTH, help\=f"Bit derinliği (varsayılan: {DEFAULT\_LSB\_DEPTH})")

parser\_stego\_extract.add\_argument("--channels", type\=int, default\=DEFAULT\_LSB\_CHANNELS, help\=f"Kullanılacak renk kanalı sayısı (varsayılan: {DEFAULT\_LSB\_CHANNELS})")

parser\_stego\_extract.add\_argument("--password", help\="Veri şifre çözme parolası")

\# Argümanları ayrıştır

args \= parser.parse\_args()

if not args.command:

parser.print\_help()

return

try:

if args.command \== "info":

bmp \= BMPFile(args.file)

info \= bmp.get\_info()

print(f"BMP Dosya Bilgisi: {info\['file\_name'\]}")

print(f"Boyut: {info\['file\_size'\]} bayt")

print(f"Başlık tipi: {info\['header\_type'\]}")

print(f"Boyutlar: {info\['dimensions'\]}")

print(f"Renk derinliği: {info\['bit\_depth'\]} bit")

print(f"Sıkıştırma: {info\['compression'\]}")

if info\['has\_metadata'\]:

print(f"Metadata: Var (Anahtarlar: {', '.join(info\['metadata\_keys'\])})")

else:

print("Metadata: Yok")

elif args.command \== "metadata":

if args.metadata\_command \== "add":

bmp \= BMPFile(args.file)

\# Mevcut metadata'yı yükle veya yeni oluştur

metadata \= bmp.extract\_metadata() or Metadata()

\# Yeni metadata ekle

metadata.add(args.key, args.value)

\# BMP'ye metadata ekle

bmp.add\_metadata(metadata, password\=args.password)

\# Kaydet

output\_path \= args.output or args.file

bmp.save(output\_path)

print(f"Metadata eklendi: {args.key}\={args.value}")

print(f"Dosya kaydedildi: {output\_path}")

elif args.metadata\_command \== "extract":

bmp \= BMPFile(args.file)

metadata \= bmp.extract\_metadata(password\=args.password)

if metadata:

print("BMP Metadata:")

for key, value in metadata.entries.items():

print(f" {key}: {value}")

else:

print("Metadata bulunamadı")

else:

parser\_metadata.print\_help()

elif args.command \== "stego":

if args.stego\_command \== "hide":

bmp \= BMPFile(args.file)

stego \= LSBSteganography(bmp)

if args.text:

\# Metin gizle

stego.hide\_text(args.text, bit\_depth\=args.bit\_depth,

channels\=args.channels, password\=args.password)

print(f"Metin mesajı gizlendi ({len(args.text)} karakter)")

elif args.hide\_file:

\# Dosya gizle

with open(args.hide\_file, "rb") as f:

file\_data \= f.read()

stego.hide\_data(file\_data, bit\_depth\=args.bit\_depth,

channels\=args.channels, password\=args.password)

print(f"Dosya gizlendi: {args.hide\_file} ({len(file\_data)} bayt)")

\# Kaydet

bmp.save(args.output)

print(f"Steganografi uygulanmış BMP kaydedildi: {args.output}")

elif args.stego\_command \== "extract":

bmp \= BMPFile(args.file)

stego \= LSBSteganography(bmp)

extracted\_data \= stego.extract\_data(bit\_depth\=args.bit\_depth,

channels\=args.channels,

password\=args.password)

if args.output:

\# Veriyi dosyaya kaydet

with open(args.output, "wb") as f:

f.write(extracted\_data)

print(f"Çıkarılan veri dosyaya kaydedildi: {args.output} ({len(extracted\_data)} bayt)")

else:

\# Veriyi metin olarak yazdırmayı dene

try:

text \= extracted\_data.decode("utf-8")

print("Çıkarılan metin:")

print(text)

except UnicodeDecodeError:

print(f"Çıkarılan veri metin değil. Boyut: {len(extracted\_data)} bayt")

print("Veriyi kaydetmek için --output parametresini kullanın")

else:

parser\_stego.print\_help()

else:

parser.print\_help()

except Exception as e:

print(f"HATA: {e}")

return 1

return 0

if \_\_name\_\_ \== "\_\_main\_\_":

sys.exit(main())