# Pemrograman Mobile - Pertemuan 14 (RESTful API)

**Nama : Rafa Fadil Aras**

**NIM  : 2341720007**

## Praktikum 1 - Membuat layanan Mock API

### Langkah-langkah praktikum 

- Langkah 1 - Daftar layanan Mock Lab
- Langkah 2 - Masuk ke layanan tersebut
- Langkah 3 - New stub
- Langkah 4 - Save stub
  ![new Stub](img/addNew.png)
- Langkah 5 - Menambahkan dependensi
  ```dart
  flutter pub add http
  ```
- Langkah 6 - File baru httphelper.dart
- Langkah 7 - Menambahkan kode 
  ```dart
  import 'dart:io'; 
  import 'package:http/http.dart' as http; 
  import 'dart:convert'; 
  import '/model/pizza.dart'; 

  class HttpHelper {
      final String authority = 'yk5yo.wiremockapi.cloud';
      final String path = '/pizzalist';
    Future<List<Pizza>> getPizzaList() async {
      final Uri url = Uri.https(authority, path);
      final http.Response result = await http.get(url);
      if (result.statusCode == HttpStatus.ok) {
        final jsonResponse = json.decode(result.body);
        //provide a type argument to the map method to avoid type 
        //error
        List<Pizza> pizzas =
            jsonResponse.map<Pizza>((i) => 
              Pizza.fromJson(i)).toList();
        return pizzas;
      } else {
        return [];
      }
    }
  }
  ```
- Langkah 8 - Menambahkan method callPizzas
  ```dart
  Future<List<Pizza>> callPizzas() async {
    HttpHelper helper = HttpHelper();
    List<Pizza> pizzas = await helper.getPizzaList();
    return pizzas;
  }
  ```
- Langkah 9 - Edit class _MyHomePageState (Scaffold)
  ```dart
    Widget build(BuildContext context) { 
      return Scaffold(
        appBar: AppBar(title: const Text('JSON')),
        body: FutureBuilder(
            future: callPizzas(),
            builder: (BuildContext context, AsyncSnapshot<List<Pizza>> 
  snapshot) {
            if (snapshot.hasError) {
              return const Text('Something went wrong');
            }
            if (!snapshot.hasData) {
              return const CircularProgressIndicator();
            }
              return ListView.builder(
                  itemCount: (snapshot.data == null) ? 0 : snapshot.
  data!.length,
                  itemBuilder: (BuildContext context, int position) {
                    return ListTile(
                      title: Text(snapshot.data![position].pizzaName),
                      subtitle: Text(snapshot.data![position].
  description +
                          ' - € ' +
                          snapshot.data![position].price.toString()),
                    );
                  });
            }),
      );  
  }
  ```
- Langkah 10 - Run
  - Soal 1 
    - Menambahkan nama panggilan pada title app
      ```dart
      appBar: AppBar(title: const Text('JSON Rafa')),
      ```
    - Ganti warna
      ```dart
      theme: ThemeData(primarySwatch: Colors.pink),
      ```
    - Capture hasil 

      ![hasilPrak1](img/hasilPrak1.png)

