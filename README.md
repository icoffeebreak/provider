# provider selector

import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

class ProductList extends StatefulWidget {
  const ProductList({super.key});

  @override
  State<ProductList> createState() => _ProductListState();
}

class _ProductListState extends State<ProductList> {
  @override
  Widget build(BuildContext context) {
    return Selector<ProductListModel, List<Product>>(
      selector: (context, productList) => productList.products,
      builder: (context, products, child) {
        return ListView.builder(
          itemCount: products.length,
          itemBuilder: (context, index) {
            final product = products[index];
            return ListTile(
              title: Text(product.name),
              subtitle: Text('\$${product.price.toStringAsFixed(2)}'),
              trailing: IconButton(
                icon: Icon(Icons.delete),
                onPressed: () {
                  context.read<ProductListModel>().removeProduct(product);
                },
              ),
            );
          },
        );
      },
    );
  }
}

class Product {
  final String name;
  final double price;

  Product(this.name, this.price);
}

class ProductListModel extends ChangeNotifier {
  List<Product> _products = [];

  List<Product> get products => _products;

  void addProduct(Product product) {
    _products.add(product);
    notifyListeners();
  }

  void removeProduct(Product product) {
    _products.remove(product);
    notifyListeners();
  }

  void _addProduct(BuildContext context) {
    final productList = context.read<ProductListModel>();
    final newProduct =
        Product("New Product", 19.99); // You can customize the product details
    productList.addProduct(newProduct);
  }
}
