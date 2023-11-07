const fs = require('fs');

class ProductManager {
  constructor(filePath) {
    this.path = filePath;
    this.products = [];
    this.loadProductsFromFile();
  }

  generateUniqueId() {
    return '_' + Math.random().toString(36).substr(2, 9);
  }

  loadProductsFromFile() {
    try {
      const data = fs.readFileSync(this.path, 'utf8');
      this.products = JSON.parse(data);
    } catch (error) {
      // Handle file not found or other errors
      this.products = [];
    }
  }

  saveProductsToFile() {
    const data = JSON.stringify(this.products, null, 2);
    fs.writeFileSync(this.path, data, 'utf8');
  }

  addProduct(productData) {
    const product = { id: this.generateUniqueId(), ...productData };
    this.products.push(product);
    this.saveProductsToFile();
    return product;
  }

  getProducts() {
    return this.products;
  }

  getProductById(id) {
    const product = this.products.find((product) => product.id === id);
    if (!product) {
      throw new Error('Producto no encontrado.');
    }
    return product;
  }

  updateProduct(id, updatedFields) {
    const productIndex = this.products.findIndex((product) => product.id === id);
    if (productIndex === -1) {
      throw new Error('Producto no encontrado.');
    }
    this.products[productIndex] = { ...this.products[productIndex], ...updatedFields };
    this.saveProductsToFile();
  }

  deleteProduct(id) {
    const productIndex = this.products.findIndex((product) => product.id === id);
    if (productIndex === -1) {
      throw new Error('Producto no encontrado.');
    }
    this.products.splice(productIndex, 1);
    this.saveProductsToFile();
  }
}

// Ejemplo de uso:
const productManager = new ProductManager('products.json');

const newProduct = productManager.addProduct({
  title: 'Producto de Ejemplo',
  description: 'Descripción de ejemplo',
  price: 100,
  thumbnail: 'ruta_de_imagen.jpg',
  code: 'abc123',
  stock: 50,
});

console.log('Producto agregado:', newProduct);

const updatedProductData = {
  title: 'Producto Actualizado',
  price: 150,
};

productManager.updateProduct(newProduct.id, updatedProductData);

const updatedProduct = productManager.getProductById(newProduct.id);
console.log('Producto actualizado:', updatedProduct);

try {
  productManager.deleteProduct(newProduct.id);
  console.log('Producto eliminado.');
} catch (error) {
  console.error('Error al eliminar producto:', error.message);
}
