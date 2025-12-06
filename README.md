def calculate_bill(bill_text):
    lines = bill_text.split('\n')
    subtotal = 0.0
    tip_percent = 0.0
    voucher_amount = 0.0

    for line in lines:
        line = line.strip()

        if "#" in line and "@" in line and "$" in line:
            quantity_part = line.split('#')[1].strip()
            quantity_str, price_str = quantity_part.split('@')

            quantity = int(quantity_str.strip())
            price = float(price_str.strip().replace("$", ""))

            subtotal += quantity * price

        elif line[:4] == "TIP:":
            tip_percent = float(line.replace("TIP:", "").replace("%", "").strip())

        elif line[:8] == "VOUCHER:":
            voucher_amount = float(line.replace("VOUCHER:", "").replace("$", "").strip())

    amount_after_voucher = subtotal - voucher_amount
    if amount_after_voucher < 0:
        amount_after_voucher = 0

    total = amount_after_voucher * (1 + tip_percent / 100)

    return f"${total:.2f}"


bill1 = """Burger # 2 @ $12.50
Fries # 2 @ $4.00
TIP: 15%
VOUCHER: $5.00"""
print(calculate_bill(bill1))  

bill2 = """Steak # 1 @ $30.00
Wine # 1 @ $10.00
TIP: 20%"""
print(calculate_bill(bill2)) 

bill3 = """Pizza # 1 @ $18.00
Soda # 2 @ $2.50
VOUCHER: $3.00
TIP: 0%"""
print(calculate_bill(bill3))
