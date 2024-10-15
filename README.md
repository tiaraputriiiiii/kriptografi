# kriptografi

Nama    : Tiara Putri

NIM     : 312210064

```
# Membuat matriks 5x5 untuk Playfair Cipher
def create_matrix(keyword):
    keyword = keyword.upper().replace('J', 'I')  # Ganti J dengan I
    seen = set()
    matrix = []

    # Tambahkan huruf dari keyword (tanpa duplikasi)
    for char in keyword:
        if char not in seen and char.isalpha():
            seen.add(char)
            matrix.append(char)

    # Tambahkan sisa huruf alfabet
    for char in "ABCDEFGHIKLMNOPQRSTUVWXYZ":  # Tanpa J
        if char not in seen:
            seen.add(char)
            matrix.append(char)

    # Konversi list ke matriks 5x5
    return [matrix[i:i + 5] for i in range(0, 25, 5)]

# Mencari koordinat suatu huruf dalam matriks
def find_position(matrix, char):
    for i, row in enumerate(matrix):
        if char in row:
            return i, row.index(char)

# Memproses teks menjadi pasangan (digram) dan tambahkan X jika perlu
def prepare_text(text):
    text = text.upper().replace('J', 'I').replace(' ', '')
    digrams = []
    i = 0

    while i < len(text):
        a = text[i]
        b = text[i + 1] if i + 1 < len(text) else 'X'

        if a == b:  # Jika huruf berulang, tambahkan X
            digrams.append((a, 'X'))
            i += 1
        else:
            digrams.append((a, b))
            i += 2

    if len(text) % 2 != 0:  # Jika ganjil, tambahkan X di akhir
        digrams.append((text[-1], 'X'))

    return digrams

# Fungsi utama untuk enkripsi Playfair Cipher
def encrypt(text, keyword):
    matrix = create_matrix(keyword)
    digrams = prepare_text(text)
    encrypted_text = ""

    for a, b in digrams:
        row1, col1 = find_position(matrix, a)
        row2, col2 = find_position(matrix, b)

        if row1 == row2:  # Jika di baris yang sama
            encrypted_text += matrix[row1][(col1 + 1) % 5]
            encrypted_text += matrix[row2][(col2 + 1) % 5]
        elif col1 == col2:  # Jika di kolom yang sama
            encrypted_text += matrix[(row1 + 1) % 5][col1]
            encrypted_text += matrix[(row2 + 1) % 5][col2]
        else:  # Jika di posisi berbeda (persegi)
            encrypted_text += matrix[row1][col2]
            encrypted_text += matrix[row2][col1]

    return encrypted_text

# Fungsi utama untuk dekripsi Playfair Cipher
def decrypt(text, keyword):
    matrix = create_matrix(keyword)
    digrams = [(text[i], text[i + 1]) for i in range(0, len(text), 2)]
    decrypted_text = ""

    for a, b in digrams:
        row1, col1 = find_position(matrix, a)
        row2, col2 = find_position(matrix, b)

        if row1 == row2:  # Jika di baris yang sama
            decrypted_text += matrix[row1][(col1 - 1) % 5]
            decrypted_text += matrix[row2][(col2 - 1) % 5]
        elif col1 == col2:  # Jika di kolom yang sama
            decrypted_text += matrix[(row1 - 1) % 5][col1]
            decrypted_text += matrix[(row2 - 1) % 5][col2]
        else:  # Jika di posisi berbeda (persegi)
            decrypted_text += matrix[row1][col2]
            decrypted_text += matrix[row2][col1]

    return decrypted_text

# Fungsi untuk memproses dan menampilkan hasil enkripsi dan dekripsi
def process_and_display(sentences, keyword):
    for sentence in sentences:
        # Enkripsi
        ciphertext = encrypt(sentence, keyword)
        # Dekripsi
        decrypted_text = decrypt(ciphertext, keyword)

        print(f"Plaintext: {sentence}")
        print(f"Ciphertext: {ciphertext}")
        print(f"Dekripsi: {decrypted_text}\n")

# Contoh penggunaan
if __name__ == "__main__":
    keyword = "TEKNIK INFORMATIKA"
    sentences = [
        "GOOD BROOM SWEEP CLEAN",
        "REDWOOD NATIONAL STATE PARK",
        "JUNK FOOD AND HEALTH PROBLEMS"
    ]

    # Proses dan tampilkan hasil
    process_and_display(sentences, keyword)
```

```
Plaintext: GOOD BROOM SWEEP CLEAN

Enkrip: CMRCDFRWRAPYKWOWBPIOKYKY

Dekrip: GOODBROXOMSWEXEPCLEANXNX
```
```
Plaintext: REDWOOD NATIONAL STATE PARK

Enkrip: OKCXRWRCIMETMEFULNFIOWFMRK

Dekrip: REDWOXODNATIONALSTATEPARKX
```
```
Plaintext: JUNK FOOD AND HEALTH PROBLEMS

Enkrip: AZINORRCMIGBIOVFCUMRLVNOQYQY

Dekrip: JUNKFOODANDHEALTHPROBLEMSXSX
```
![Screenshot 2024-10-15 091542](https://github.com/user-attachments/assets/6e29f1ea-c7f8-44a0-9f0f-a0d046265eb3)
