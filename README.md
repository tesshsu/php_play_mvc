# php_play_mvc
This is play for login use sql select, insert, update and delet user data


# work flow
scenerio: delete one user from database and view
require files:
libs/Database.php
models/user_model.php
controller/user.php
views/user/index.php

Actions
Libs/Database.php
Class Database extends PDO::delet($table, $where, $limit = 1);
// defined abstract @param string $table, string $where, integer $limit
public function delete($table, $where, $limit = 1)
	{
		return $this->exec("DELETE FROM $table WHERE $where LIMIT $limit");
	}

views/user/index.php
write a form, under this form, it has show userList with Edit/Delete button


controller/user.php
Class extends libs/Controller which defined the model path
Write public delete function which call model method delete, and direct to view user page
class User extends Controller {
  public function delete($id)
    {
      $this->model->delete($id);
      header('location: ' . URL . 'user');
    }
}


models/user_model.php
Class extends Model which had defined Database constant
Excute libs/Database.php selest method
Defined if role is owner delete method will invalide
Excute libs/Database.php delete method
class User_Model extends Model
{
  public function delete($id)
    {
      $result = $this->db->select('SELECT role FROM user WHERE id = :id', array(':id' => $id));

      if ($result[0]['role'] == 'owner')
      return false;

      $this->db->delete('user', "id = '$id'");
    }
}

