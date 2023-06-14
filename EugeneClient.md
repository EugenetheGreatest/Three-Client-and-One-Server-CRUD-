# Three-Client-and-One-Server-CRUD-
IAS 2 Examination
//ADD EugeneClient
Imports MySql.Data.MySqlClient
Public Class register
    Dim connString As String = "server=192.168.18.100;user=root;password=;database=uge"
    Dim conn As New MySqlConnection(connString)
    Private Sub Button1_Click(sender As Object, e As EventArgs) Handles Button1.Click
        Dim username As String = TextBox1.Text
        Dim password As String = TextBox2.Text
        Dim id As Integer = TextBox3.Text

        Try
            conn.Open()
            Dim query As String = "INSERT INTO tbl_uge (id, name, password) VALUES (@id, @name, @password)"
            Dim command As MySqlCommand = New MySqlCommand(query, conn)
            command.Parameters.AddWithValue("@id", id)
            command.Parameters.AddWithValue("@name", username)
            command.Parameters.AddWithValue("@password", password)
            command.ExecuteNonQuery()

            MessageBox.Show("SUCCESS!")
            TextBox1.Clear()
            TextBox2.Clear()
            TextBox3.Clear()
        Catch ex As Exception
            MessageBox.Show("Error occurred: " & ex.Message)
        Finally
            conn.Close()
        End Try
        LoadData()

    End Sub

    Private Sub DataGridView1_CellContentClick(sender As Object, e As DataGridViewCellEventArgs) Handles DataGridView1.CellContentClick
        If e.ColumnIndex = DataGridView1.Columns("Action").Index AndAlso e.RowIndex >= 0 Then
            Dim rowIndex As Integer = e.RowIndex
        End If
    End Sub

    Private Sub Button2_Click(sender As Object, e As EventArgs) Handles Button2.Click
        If DataGridView1.SelectedRows.Count > 0 Then
            Dim selectedRow As DataGridViewRow = DataGridView1.SelectedRows(0)
            Dim id As String = selectedRow.Cells("id").Value.ToString()
            Dim name As String = selectedRow.Cells("name").Value.ToString()
            Dim password As String = selectedRow.Cells("password").Value.ToString()
            Dim updateUser As New updateUser()
            updateUser.TextBox1.Text = name
            updateUser.TextBox2.Text = password
            updateUser.TextBox3.Text = id
            updateUser.ShowDialog()
        Else
            MessageBox.Show("Please select a user to update.")
        End If
    End Sub
    Private Sub LoadData()
        Try
            conn.Open()

            Dim query As String = "SELECT * FROM tbl_uge"
            Dim command As MySqlCommand = New MySqlCommand(query, conn)
            Dim adapter As MySqlDataAdapter = New MySqlDataAdapter(command)
            Dim dataTable As DataTable = New DataTable()
            adapter.Fill(dataTable)
            DataGridView1.DataSource = dataTable

        Catch ex As Exception
            MessageBox.Show("Error occurred: " & ex.Message)
        Finally
            conn.Close()
        End Try
    End Sub

    Private Sub register_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        Try
            conn.Open()

            Dim query As String = "SELECT * FROM tbl_uge"
            Dim command As MySqlCommand = New MySqlCommand(query, conn)
            Dim adapter As MySqlDataAdapter = New MySqlDataAdapter(command)
            Dim dataTable As DataTable = New DataTable()
            adapter.Fill(dataTable)

            DataGridView1.DataSource = dataTable

            DataGridView1.AutoSizeColumnsMode = DataGridViewAutoSizeColumnsMode.Fill

        Catch ex As Exception
            MessageBox.Show("Error occurred: " & ex.Message)
        Finally
            conn.Close()
        End Try
    End Sub

    Private Sub Button3_Click(sender As Object, e As EventArgs) Handles Button3.Click
        LoadData()
    End Sub
End Class

//UPDATE EugeneClient
Imports MySql.Data.MySqlClient
Public Class updateUser
    Private Sub Button1_Click(sender As Object, e As EventArgs) Handles Button1.Click
        Dim newUsername As String = TextBox1.Text
        Dim newPassword As String = TextBox2.Text

        Dim connectionString As String = "server=192.168.18.100;user=root;password=;database=uge"
        'Dim query As String = $"UPDATE tbl_uge SET name = '{TextBox1.Text}', password = '{TextBox2.Text}'"
        Dim query As String = "UPDATE tbl_uge SET name='" & TextBox1.Text & "', password='" & TextBox2.Text & "'WHERE ID = '" & TextBox3.Text & "'"

        Using connection As New MySqlConnection(connectionString)
            connection.Open()
            Using command As New MySqlCommand(query, connection)
                command.ExecuteNonQuery()
            End Using
        End Using


        Me.Close()
    End Sub

    Private Sub updateUser_Load(sender As Object, e As EventArgs) Handles MyBase.Load

    End Sub
End Class

//DELETE EugeneClient2
Imports MySql.Data.MySqlClient
Public Class Form1
    Dim connString As String = "server=192.168.18.100;user=root;password=;database=uge"
    Dim conn As New MySqlConnection(connString)
    Private Sub Button1_Click(sender As Object, e As EventArgs) Handles Button1.Click

        If DataGridView1.SelectedRows.Count > 0 Then 'check if user selected a row
            'Retrieve the selected user's id from the DataGridView
            Dim selectedRow As DataGridViewRow = DataGridView1.SelectedRows(0)
            Dim id As Integer = Integer.Parse(selectedRow.Cells("id").Value.ToString())

            'Construct the delete query
            Dim query As String = $"DELETE FROM tbl_uge WHERE id = {id}"

            'Execute the delete query
            Dim connectionString As String = "server=localhost;user=root;password=;database=uge"
            Using connection As New MySqlConnection(connectionString)
                connection.Open()
                Using command As New MySqlCommand(query, connection)
                    command.ExecuteNonQuery()
                End Using
            End Using

            'Remove the selected row from the DataGridView
            DataGridView1.Rows.Remove(selectedRow)

            MessageBox.Show("User deleted successfully.")
        Else
            MessageBox.Show("Please select a user to delete.")
        End If
    End Sub

    Private Sub Button2_Click(sender As Object, e As EventArgs) Handles Button2.Click
        LoadData()
    End Sub
    Private Sub LoadData()
        Try
            conn.Open()

            Dim query As String = "SELECT * FROM tbl_uge"
            Dim command As MySqlCommand = New MySqlCommand(query, conn)
            Dim adapter As MySqlDataAdapter = New MySqlDataAdapter(command)
            Dim dataTable As DataTable = New DataTable()
            adapter.Fill(dataTable)
            DataGridView1.DataSource = dataTable

        Catch ex As Exception
            MessageBox.Show("Error occurred: " & ex.Message)
        Finally
            conn.Close()
        End Try
    End Sub

    Private Sub Form1_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        Try
            conn.Open()

            Dim query As String = "SELECT * FROM tbl_uge"
            Dim command As MySqlCommand = New MySqlCommand(query, conn)
            Dim adapter As MySqlDataAdapter = New MySqlDataAdapter(command)
            Dim dataTable As DataTable = New DataTable()
            adapter.Fill(dataTable)

            DataGridView1.DataSource = dataTable

            DataGridView1.AutoSizeColumnsMode = DataGridViewAutoSizeColumnsMode.Fill

        Catch ex As Exception
            MessageBox.Show("Error occurred: " & ex.Message)
        Finally
            conn.Close()
        End Try
    End Sub

End Class

//READ EugeneClient3
Imports MySql.Data.MySqlClient
Public Class Form1
    Dim connString As String = "server=192.168.18.100;user=root;password=;database=uge"
    Dim conn As New MySqlConnection(connString)
    Private Sub Button1_Click(sender As Object, e As EventArgs) Handles Button1.Click
        LoadData()
    End Sub
    Private Sub LoadData()
        Try
            conn.Open()

            Dim query As String = "SELECT * FROM tbl_uge"
            Dim command As MySqlCommand = New MySqlCommand(query, conn)
            Dim adapter As MySqlDataAdapter = New MySqlDataAdapter(command)
            Dim dataTable As DataTable = New DataTable()
            adapter.Fill(dataTable)
            DataGridView1.DataSource = dataTable

        Catch ex As Exception
            MessageBox.Show("Error occurred: " & ex.Message)
        Finally
            conn.Close()
        End Try
    End Sub

    Private Sub Form1_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        Try
            conn.Open()

            Dim query As String = "SELECT * FROM tbl_uge"
            Dim command As MySqlCommand = New MySqlCommand(query, conn)
            Dim adapter As MySqlDataAdapter = New MySqlDataAdapter(command)
            Dim dataTable As DataTable = New DataTable()
            adapter.Fill(dataTable)

            DataGridView1.DataSource = dataTable

            DataGridView1.AutoSizeColumnsMode = DataGridViewAutoSizeColumnsMode.Fill

        Catch ex As Exception
            MessageBox.Show("Error occurred: " & ex.Message)
        Finally
            conn.Close()
        End Try
    End Sub
End Class

