<h3>Cấu trúc tổ chức project sử dụng mô hình MVVM trong Flutter</h3>
MVVM (Model-View-ViewModel) là một mô hình thiết kế phần mềm được sử dụng để xử lý dữ liệu và giao diện người dùng trong Flutter. Mô hình này chia nhỏ logic của ứng dụng thành ba lớp riêng biệt: Model, View và ViewModel.

Model: Lớp này lưu trữ dữ liệu và tạo ra các cách để tương tác với dữ liệu.
View: Lớp này là giao diện người dùng và chịu trách nhiệm hiển thị dữ liệu cho người dùng.
ViewModel: Lớp này được sử dụng để trung gian giữa Model và View. Nó chuyển đổi dữ liệu từ Model thành một định dạng có thể hiển thị trên View và chịu trách nhiệm xử lý các hoạt động như gọi API và cập nhật Model với dữ liệu được lấy từ API.
MVVM là mô hình rất tốt cho Flutter vì nó giúp tách rời logic của ứng dụng ra khỏi giao diện người dùng và giúp cho mã code trở nên dễ dàng quản lý và bảo trì hơn.

Dưới đây là cấu trúc folder tổ chức project sử dụng MVVM:

lib
  |- models (folder for model classes)
  |   |- user.dart (representation of user data)
  |
  |- viewmodels (folder for view model classes)
  |   |- user_view_model.dart (converts user data into a format that can be displayed and processes API calls)
  |
  |- views (folder for widget classes)
  |   |- user_card.dart (displays user data)
  |
  |- api (folder for API classes)
  |   |- user_api.dart (handles API calls related to user data)
  |
  |- main.dart (entry point of the application)
Model là class user:

// models/user.dart
class User {
  final String name;
  final String email;

  User({this.name, this.email});
}
View là widget hiển thị thông tin user trên trên giao diện:

// views/user_card.dart
import 'package:flutter/widgets.dart';
import 'package:flutter_mvvm_structure_with_api/viewmodels/user_view_model.dart';

class UserCard extends StatelessWidget {
  final UserViewModel viewModel;

  UserCard({this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Container(
      child: Column(
        children: <Widget>[
          Text(viewModel.user.name),
          Text(viewModel.user.email),
        ],
      ),
    );
  }
}
ViewModel là lớp trung gian giữ model user và view thông tin user làm nhiệm vụ gọi API:

// viewmodels/user_view_model.dart
import 'package:flutter_mvvm_structure_with_api/models/user.dart';
import 'package:flutter_mvvm_structure_with_api/api/user_api.dart';

class UserViewModel {
  User _user;

  User get user => _user;

  Future<void> fetchUser(String id) async {
    _user = await UserApi.fetchUser(id);
  }
}
// api/user_api.dart
import 'dart:convert';
import 'package:http/http.dart' as http;

class UserApi {
  static Future<User> fetchUser(String id) async {
    final response = await http.get('https://your-api-endpoint.com/users/$id');

    if (response.statusCode == 200) {
      final userData = json.decode(response.body);
      return User(
        name: userData['name'],
        email: userData['email'],
      );
    } else {
      throw Exception('Failed to fetch user data');
    }
  }
}
<h5>link about document this here: https://tradacongnghe.com/2023/02/01/cau-truc-to-chuc-project-su-dung-mo-hinh-mvvm-trong-flutter/</h5>
