import random

# Таблиця для S-блоку
S_BLOCK_TABLE = {
    0b0000: 0b1110, 0b0001: 0b0100, 0b0010: 0b1101, 0b0011: 0b0001,
    0b0100: 0b0010, 0b0101: 0b1111, 0b0110: 0b1011, 0b0111: 0b1000,
    0b1000: 0b0011, 0b1001: 0b1010, 0b1010: 0b0110, 0b1011: 0b1100,
    0b1100: 0b0101, 0b1101: 0b1001, 0b1110: 0b0000, 0b1111: 0b0111,
}

# Створимо зворотну таблицю для S-блоку
S_BLOCK_REVERSE_TABLE = {v: k for k, v in S_BLOCK_TABLE.items()}

# Функція для S-блоку
def s_block_encrypt(data):
    high_nibble = (data >> 4) & 0b1111
    low_nibble = data & 0b1111
    return (S_BLOCK_TABLE[high_nibble] << 4) | S_BLOCK_TABLE[low_nibble]

def s_block_decrypt(data):
    high_nibble = (data >> 4) & 0b1111
    low_nibble = data & 0b1111
    return (S_BLOCK_REVERSE_TABLE[high_nibble] << 4) | S_BLOCK_REVERSE_TABLE[low_nibble]

# Формула для перестановки бітів у P-блоці
P_BLOCK_PERMUTATION = [7, 6, 5, 4, 3, 2, 1, 0]

# Функція для P-блоку
def p_block_permute(data):
    permuted_data = 0
    for i, bit_position in enumerate(P_BLOCK_PERMUTATION):
        bit = (data >> bit_position) & 1
        permuted_data |= (bit << (7 - i))
    return permuted_data

def p_block_reverse_permute(data):
    reverse_data = 0
    for i, bit_position in enumerate(P_BLOCK_PERMUTATION):
        bit = (data >> (7 - i)) & 1
        reverse_data |= (bit << bit_position)
    return reverse_data

# Тестування
data = 0b10101100
encrypted_s_block = s_block_encrypt(data)
decrypted_s_block = s_block_decrypt(encrypted_s_block)
print(f"S-блок: Вхідні дані = {bin(data)}, Зашифровані = {bin(encrypted_s_block)}, Розшифровані = {bin(decrypted_s_block)}")

permuted_p_block = p_block_permute(data)
reversed_p_block = p_block_reverse_permute(permuted_p_block)
print(f"P-блок: Вхідні дані = {bin(data)}, Переставлені = {bin(permuted_p_block)}, Зворотня перестановка = {bin(reversed_p_block)}")
