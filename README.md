class TechShop:
    def __init__(self):
    
        self.products = [
            {"id": 1, "name": "Ноутбук", "price": 25000},
            {"id": 2, "name": "Смартфон", "price": 15000},
            {"id": 3, "name": "Телевізор", "price": 30000}
        ]
        self.cart = []
        self.address = ""
        self.payment_method = ""
        self.order_history = []  # Історія замовлень

    def show_products(self):
        while True:
            print("\nДоступна техніка:")
            for item in self.products:
                print(f'{item["id"]}. {item["name"]} - {item["price"]} грн')
            print("\nВведіть ID техніки для додавання до корзини або '0' для виходу:")
            choice = input("Ваш вибір: ")
            if choice == "0":
                break
            else:
                try:
                    item_id = int(choice)
                    self.add_to_cart(item_id)
                except ValueError:
                    print("Неправильний ввід. Будь ласка, введіть число.")

    def add_to_cart(self, item_id):
        item = next((x for x in self.products if x["id"] == item_id), None)
        if item:
            self.cart.append(item)
            print(f'{item["name"]} додано до корзини.')
        else:
            print("Неправильний ID техніки.")

    def show_cart(self):
        if not self.cart:
            print("Ваша корзина порожня.")
        else:
            print("\nВаш кошик:")
            for item in self.cart:
                print(f'{item["name"]} - {item["price"]} грн')
            print("Введіть ID товару для видалення з корзини або '0' для виходу:")
            choice = input("Ваш вибір: ")
            if choice != "0":
                try:
                    item_id = int(choice)
                    self.remove_from_cart(item_id)
                except ValueError:
                    print("Неправильний ввід. Будь ласка, введіть число.")

    def remove_from_cart(self, item_id):
        item = next((x for x in self.cart if x["id"] == item_id), None)
        if item:
            self.cart.remove(item)
            print(f'{item["name"]} видалено з корзини.')
        else:
            print("Неправильний ID техніки.")

    def enter_address(self):
        self.address = input("Введіть адресу доставки: ")

    def choose_payment_method(self):
        print("Виберіть спосіб оплати:")
        print("1. Картка")
        print("2. Готівка при отриманні")
        choice = input("Введіть номер вибраного способу оплати: ")
        if choice == "1":
            self.payment_method = "Картка"
        elif choice == "2":
            self.payment_method = "Готівка при отриманні"
        else:
            print("Неправильний вибір.")

    def checkout(self):
        if not self.cart:
            print("Ваша корзина порожня.")
            return

        self.enter_address()
        self.choose_payment_method()

        if not self.address:
            print("Введіть адресу доставки.")
            return
        if not self.payment_method:
            print("Виберіть спосіб оплати.")
            return

        print("\nПідсумок замовлення:")
        total = 0
        for item in self.cart:
            print(f'{item["name"]} - {item["price"]} грн')
            total += item["price"]
        print(f'Загальна сума: {total} грн')
        print(f'Адреса доставки: {self.address}')
        print(f'Спосіб оплати: {self.payment_method}')
        print("Дякуємо за ваше замовлення!")

        order = {
            "items": self.cart.copy(),
            "total": total,
            "address": self.address,
            "payment_method": self.payment_method
        }
        self.order_history.append(order)
        self.cart.clear()

    def add_new_product(self):
        print("Додавання нового товару:")
        try:
            name = input("Введіть назву товару: ")
            price = int(input("Введіть ціну товару (грн): "))
            new_id = max([item["id"] for item in self.products]) + 1
            new_item = {"id": new_id, "name": name, "price": price}
            self.products.append(new_item)
            print(f'{name} додано до асортименту.')
        except ValueError:
            print("Неправильний ввід. Ціна повинна бути числом.")

    def show_order_history(self):
        if not self.order_history:
            print("Історія замовлень порожня.")
        else:
            print("\nІсторія замовлень:")
            for idx, order in enumerate(self.order_history):
                print(f"\nЗамовлення #{idx + 1}:")
                for item in order["items"]:
                    print(f'{item["name"]} - {item["price"]} грн')
                print(f'Загальна сума: {order["total"]} грн')
                print(f'Адреса доставки: {order["address"]}')
                print(f'Спосіб оплати: {order["payment_method"]}')

    def search_product_by_name(self, name):
        results = [item for item in self.products if name.lower() in item["name"].lower()]
        if results:
            print("\nРезультати пошуку:")
            for item in results:
                print(f'{item["id"]}. {item["name"]} - {item["price"]} грн')
        else:
            print("Товар не знайдено.")

    def edit_product(self, item_id):
        item = next((x for x in self.products if x["id"] == item_id), None)
        if item:
            print(f'Редагування товару: {item["name"]}')
            try:
                new_name = input(f'Нова назва ({item["name"]}): ')
                new_price = input(f'Нова ціна ({item["price"]} грн): ')
                if new_name:
                    item["name"] = new_name
                if new_price:
                    item["price"] = int(new_price)
                print("Товар успішно відредаговано.")
            except ValueError:
                print("Неправильний ввід. Ціна повинна бути числом.")
        else:
            print("Товар з таким ID не знайдено.")

    def show_order_details(self, order_id):
        if 0 <= order_id < len(self.order_history):
            order = self.order_history[order_id]
            print("\nДеталі замовлення:")
            for item in order["items"]:
                print(f'{item["name"]} - {item["price"]} грн')
            print(f'Загальна сума: {order["total"]} грн')
            print(f'Адреса доставки: {order["address"]}')
            print(f'Спосіб оплати: {order["payment_method"]}')
        else:
            print("Замовлення з таким номером не знайдено.")

    def show_cheapest_products(self):
        cheapest_items = sorted(self.products, key=lambda x: x["price"])[:3]
        print("\nНайменш дорогі товари:")
        for item in cheapest_items:
            print(f'{item["id"]}. {item["name"]} - {item["price"]} грн')

    def sort_products_by_price(self, ascending=True):
        sorted_products = sorted(self.products, key=lambda x: x["price"], reverse=not ascending)
        print("\nТовари, відсортовані за ціною:")
        for item in sorted_products:
            print(f'{item["id"]}. {item["name"]} - {item["price"]} грн')

    def total_sales_amount(self):
        total_sales = sum(order["total"] for order in self.order_history)
        print(f'\nЗагальна сума всіх замовлень: {total_sales} грн')

def main():
    shop = TechShop()

    while True:
        print("\nМеню:")
        print("1. Показати техніку")
        print("2. Корзина")
        print("3. Оформити замовлення")
        print("4. Додати новий товар")
        print("5. Історія замовлень")
        print("6. Пошук товару")
        print("7. Редагувати товар")
        print("8. Деталі замовлення")
        print("9. Показати найдешевші товари")
        print("10. Сортувати товари за ціною")
        print("11. Загальна сума всіх замовлень")
        print("12. Вийти")

        choice = input("Виберіть пункт меню: ")

        if choice == "1":
            shop.show_products()
        elif choice == "2":
            shop.show_cart()
        elif choice == "3":
            shop.checkout()
        elif choice == "4":
            shop.add_new_product()
        elif choice == "5":
            shop.show_order_history()
        elif choice == "6":
            name = input("Введіть назву товару для пошуку: ")
            shop.search_product_by_name(name)
        elif choice == "7":
            try:
                item_id = int(input("Введіть ID товару для редагування: "))
                shop.edit_product(item_id)
            except ValueError:
                print("Неправильний ввід. ID повинно бути числом.")
        elif choice == "8":
            try:
                order_id = int(input("Введіть номер замовлення: "))
                shop.show_order_details(order_id)
            except ValueError:
                print("Неправильний ввід. Номер замовлення повинен бути числом.")
        elif choice == "9":
            shop.show_cheapest_products()
        elif choice == "10":
            order = input("Введіть 'asc' для сортування за зростанням або 'desc' для сортування за спаданням: ")
            if order == "asc":
                shop.sort_products_by_price(ascending=True)
            elif order == "desc":
                shop.sort_products_by_price(ascending=False)
            else:
                print("Неправильний вибір.")
        elif choice == "11":
            shop.total_sales_amount()
        elif choice == "12":
            print("До побачення!")
            break
        else:
            print("Неправильний вибір. Спробуйте ще раз.")

if __name__ == "__main__":
    main()
