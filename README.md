pdv-padaria-pro
import 'package:flutter/material.dart';

void main() {
  runApp(const PadariaPDVApp());
}

class PadariaPDVApp extends StatelessWidget {
  const PadariaPDVApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'PDV Padaria Pro',
      theme: ThemeData(
        primarySwatch: Colors.orange,
        scaffoldBackgroundColor: const Color(0xfff5f5f5),
        fontFamily: 'Roboto',
      ),
      home: const DashboardScreen(),
    );
  }
}

class Produto {
  final String nome;
  final double preco;
  final int estoque;

  Produto({
    required this.nome,
    required this.preco,
    required this.estoque,
  });
}

class DashboardScreen extends StatefulWidget {
  const DashboardScreen({super.key});

  @override
  State<DashboardScreen> createState() => _DashboardScreenState();
}

class _DashboardScreenState extends State<DashboardScreen> {
  final List<Produto> produtos = [
    Produto(nome: 'Pão Francês', preco: 1.00, estoque: 120),
    Produto(nome: 'Croissant', preco: 8.00, estoque: 30),
    Produto(nome: 'Café Expresso', preco: 5.00, estoque: 50),
    Produto(nome: 'Bolo Chocolate', preco: 15.00, estoque: 12),
    Produto(nome: 'Salgado', preco: 7.50, estoque: 40),
  ];

  final List<Produto> carrinho = [];

  double get total => carrinho.fold(0, (sum, item) => sum + item.preco);

  void adicionarCarrinho(Produto produto) {
    setState(() {
      carrinho.add(produto);
    });
  }

  void finalizarVenda() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Venda Finalizada'),
        content: Text('Total da venda: R\$ ${total.toStringAsFixed(2)}'),
        actions: [
          TextButton(
            onPressed: () {
              setState(() {
                carrinho.clear();
              });
              Navigator.pop(context);
            },
            child: const Text('OK'),
          )
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        elevation: 0,
        backgroundColor: Colors.orange,
        title: const Text('PDV Padaria Pro'),
        actions: const [
          Padding(
            padding: EdgeInsets.all(12),
            child: Icon(Icons.account_circle, size: 30),
          )
        ],
      ),
      drawer: Drawer(
        child: ListView(
          children: const [
            DrawerHeader(
              decoration: BoxDecoration(color: Colors.orange),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Icon(Icons.store, size: 60, color: Colors.white),
                  SizedBox(height: 10),
                  Text(
                    'Padaria Pro',
                    style: TextStyle(color: Colors.white, fontSize: 22),
                  )
                ],
              ),
            ),
            ListTile(
              leading: Icon(Icons.point_of_sale),
              title: Text('PDV'),
            ),
            ListTile(
              leading: Icon(Icons.inventory),
              title: Text('Estoque'),
            ),
            ListTile(
              leading: Icon(Icons.bar_chart),
              title: Text('Relatórios'),
            ),
            ListTile(
              leading: Icon(Icons.people),
              title: Text('Clientes'),
            ),
            ListTile(
              leading: Icon(Icons.settings),
              title: Text('Configurações'),
            ),
          ],
        ),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            Row(
              children: [
                Expanded(
                  child: dashboardCard(
                    'Faturamento',
                    'R\$ 5.840',
                    Icons.attach_money,
                  ),
                ),
                const SizedBox(width: 10),
                Expanded(
                  child: dashboardCard(
                    'Pedidos',
                    '148',
                    Icons.shopping_cart,
                  ),
                ),
              ],
            ),
            const SizedBox(height: 20),
            Expanded(
              child: Row(
                children: [
                  Expanded(
                    flex: 2,
                    child: Container(
                      padding: const EdgeInsets.all(16),
                      decoration: BoxDecoration(
                        color: Colors.white,
                        borderRadius: BorderRadius.circular(20),
                      ),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          const Text(
                            'Produtos',
                            style: TextStyle(
                              fontSize: 24,
                              fontWeight: FontWeight.bold,
                            ),
                          ),
                          const SizedBox(height: 20),
                          Expanded(
                            child: ListView.builder(
                              itemCount: produtos.length,
                              itemBuilder: (context, index) {
                                final produto = produtos[index];

                                return Card(
                                  elevation: 2,
                                  shape: RoundedRectangleBorder(
                                    borderRadius: BorderRadius.circular(15),
                                  ),
                                  child: ListTile(
                                    leading: const CircleAvatar(
                                      backgroundColor: Colors.orange,
                                      child: Icon(Icons.bakery_dining,
                                          color: Colors.white),
                                    ),
                                    title: Text(produto.nome),
                                    subtitle: Text(
                                      'Estoque: ${produto.estoque}',
                                    ),
                                    trailing: Column(
                                      mainAxisAlignment:
                                          MainAxisAlignment.center,
                                      children: [
                                        Text(
                                          'R\$ ${produto.preco.toStringAsFixed(2)}',
                                          style: const TextStyle(
                                            fontWeight: FontWeight.bold,
                                          ),
                                        ),
                                        ElevatedButton(
                                          onPressed: () =>
                                              adicionarCarrinho(produto),
                                          style: ElevatedButton.styleFrom(
                                            backgroundColor: Colors.orange,
                                          ),
                                          child: const Text('Adicionar'),
                                        )
                                      ],
                                    ),
                                  ),
                                );
                              },
                            ),
                          )
                        ],
                      ),
                    ),
                  ),
                  const SizedBox(width: 16),
                  Expanded(
                    child: Container(
                      padding: const EdgeInsets.all(16),
                      decoration: BoxDecoration(
                        color: Colors.white,
                        borderRadius: BorderRadius.circular(20),
                      ),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          const Text(
                            'Carrinho',
                            style: TextStyle(
                              fontSize: 24,
                              fontWeight: FontWeight.bold,
                            ),
                          ),
                          const SizedBox(height: 20),
                          Expanded(
                            child: ListView.builder(
                              itemCount: carrinho.length,
                              itemBuilder: (context, index) {
                                final item = carrinho[index];
                                return ListTile(
                                  title: Text(item.nome),
                                  trailing: Text(
                                    'R\$ ${item.preco.toStringAsFixed(2)}',
                                  ),
                                );
                              },
                            ),
                          ),
                          const Divider(),
                          Row(
                            mainAxisAlignment: MainAxisAlignment.spaceBetween,
                            children: [
                              const Text(
                                'TOTAL',
                                style: TextStyle(
                                  fontSize: 20,
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                              Text(
                                'R\$ ${total.toStringAsFixed(2)}',
                                style: const TextStyle(
                                  fontSize: 22,
                                  color: Colors.green,
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                            ],
                          ),
                          const SizedBox(height: 20),
                          SizedBox(
                            width: double.infinity,
                            child: ElevatedButton.icon(
                              onPressed: finalizarVenda,
                              icon: const Icon(Icons.pix),
                              label: const Text('Finalizar Venda'),
                              style: ElevatedButton.styleFrom(
                                backgroundColor: Colors.green,
                                padding:
                                    const EdgeInsets.symmetric(vertical: 18),
                                shape: RoundedRectangleBorder(
                                  borderRadius: BorderRadius.circular(15),
                                ),
                              ),
                            ),
                          )
                        ],
                      ),
                    ),
                  )
                ],
              ),
            )
          ],
        ),
      ),
    );
  }

  Widget dashboardCard(String titulo, String valor, IconData icone) {
    return Container(
      padding: const EdgeInsets.all(20),
      decoration: BoxDecoration(
        color: Colors.white,
        borderRadius: BorderRadius.circular(20),
      ),
      child: Row(
        children: [
          CircleAvatar(
            radius: 28,
            backgroundColor: Colors.orange.shade100,
            child: Icon(icone, color: Colors.orange, size: 30),
          ),
          const SizedBox(width: 15),
          Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                titulo,
                style: const TextStyle(color: Colors.grey),
              ),
              const SizedBox(height: 5),
              Text(
                valor,
                style: const TextStyle(
                  fontSize: 24,
                  fontWeight: FontWeight.bold,
                ),
              )
            ],
          )
        ],
      ),
    );
  }
}

/*
COMO GERAR APK:

1. Instale Flutter
2. Crie projeto:
   flutter create pdv_padaria

3. Substitua o arquivo lib/main.dart por este código.

4. Rode no Android:
   flutter run

5. Gerar APK:
   flutter build apk --release

APK FINAL:
build/app/outputs/flutter-apk/app-release.apk

PRÓXIMAS EVOLUÇÕES:
- Firebase
- Login
- Multiusuário
- Impressora térmica
- NFC-e
- Banco SQLite
- Backup online
- Dashboard avançado
- Controle financeiro completo
- Integração iFood
- QR Code PIX
*/
