using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using Npgsql;

namespace Hemeroteca
{
    public partial class Form1 : Form

    {
        string connectionString = "Host=postgres.render.com;Port=5432;Username=admin;Password=u7mIRtzwKVjfOwm8ClSbxO6VD6UDTTNd;Database=hemeroteca;SSL Mode=Require;Trust Server Certificate=true;";
        Timer refreshTimer;

        public Form1()
        {
            InitializeComponent();
            ConfigurarTimer();
            CargarDatos();
        }

        private void ConfigurarTimer()
        {
            refreshTimer = new Timer();
            refreshTimer.Interval = 5000; // Intervalo de 5 segundos
            refreshTimer.Tick += (s, e) => CargarDatos();
            refreshTimer.Start();
        }

        private void CargarDatos()
        {
            try
            {
                using (var conn = new NpgsqlConnection(connectionString))
                {
                    conn.Open();


                    string queryPublicaciones = "SELECT id, titulo, autor, estatus, fecha_publicacion FROM Publicaciones ORDER BY fecha_publicacion DESC";
                    NpgsqlDataAdapter adapter1 = new NpgsqlDataAdapter(queryPublicaciones, conn);
                    DataTable dtPublicaciones = new DataTable();
                    adapter1.Fill(dtPublicaciones);
                    dataGridViewPublicaciones.DataSource = dtPublicaciones;


                    string queryEjemplares = "SELECT id, publicacion_id, codigo, estatus, fecha_ingreso FROM ejemplares WHERE estatus = 'Prestado' ORDER BY fecha_ingreso DESC";
                    NpgsqlDataAdapter adapter2 = new NpgsqlDataAdapter(queryEjemplares, conn);
                    DataTable dtEjemplares = new DataTable();
                    adapter2.Fill(dtEjemplares);
                    dataGridViewEjemplares.DataSource = dtEjemplares;
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Error al cargar los datos: {ex.Message}");
            }
        }

        private void label1_Click(object sender, EventArgs e)
        {

        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }

        private void usuariosToolStripMenuItem_Click(object sender, EventArgs e)
        {
            Form UsuariosF = new Usuarios();
            UsuariosF.Show();
            this.Hide();
        }

        private void solicitudesToolStripMenuItem_Click(object sender, EventArgs e)
        {
            Form UsuariosF = new Prestamos();
            UsuariosF.Show();
            this.Hide();
        }

        private void inicioToolStripMenuItem_Click(object sender, EventArgs e)
        {
            Form UsuariosF = new Form1();
            UsuariosF.Show();
            this.Hide();
        }

        private void publicacionesToolStripMenuItem_Click(object sender, EventArgs e)
        {
            Form UsuariosF = new Publicaciones();
            UsuariosF.Show();
            this.Hide();
        }

        private void devolucionesToolStripMenuItem_Click(object sender, EventArgs e)
        {
            Form UsuariosF = new Devoluciones();
            UsuariosF.Show();
            this.Hide();
        }
    }
}
