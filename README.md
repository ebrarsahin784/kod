# kod
    public partial class Form1 : Form
    {
         SqlConnection baglanti = new SqlConnection("Data Source=.SQLEXPRESS;Initial Catalog=fitness;Integrated Security=True;Encrypt=False");



        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            // TODO: Bu kod satırı 'fitnessDataSet.uyelikler' tablosuna veri yükler. Bunu gerektiği şekilde taşıyabilir, veya kaldırabilirsiniz.
            this.uyeliklerTableAdapter.Fill(this.fitnessDataSet.uyelikler);
            // TODO: Bu kod satırı 'fitnessDataSet.antrenorler' tablosuna veri yükler. Bunu gerektiği şekilde taşıyabilir, veya kaldırabilirsiniz.
            this.antrenorlerTableAdapter.Fill(this.fitnessDataSet.antrenorler);
            // TODO: Bu kod satırı 'fitnessDataSet.paketler' tablosuna veri yükler. Bunu gerektiği şekilde taşıyabilir, veya kaldırabilirsiniz.
            this.paketlerTableAdapter.Fill(this.fitnessDataSet.paketler);
            // TODO: Bu kod satırı 'fitnessDataSet.uyeler' tablosuna veri yükler. Bunu gerektiği şekilde taşıyabilir, veya kaldırabilirsiniz.
            this.uyelerTableAdapter.Fill(this.fitnessDataSet.uyeler);








            // 1. PAKETLERİ MANUEL DOLDURMA
            DataTable dtPaketler = new DataTable();
            dtPaketler.Columns.Add("PaketID", typeof(int));
            dtPaketler.Columns.Add("PaketAdi", typeof(string));

            // Elindeki paketleri ID'leriyle buraya ekle:
            dtPaketler.Rows.Add(1, "1 Aylık Standart Paket");
            dtPaketler.Rows.Add(2, "3 Aylık Gold Paket");
            dtPaketler.Rows.Add(3, "6 Aylık Platin Paket");
            dtPaketler.Rows.Add(4, "12 Aylık VIP Paket");

            cmbPaketler.DataSource = dtPaketler;
            cmbPaketler.DisplayMember = "PaketAdi"; // Kullanıcı adı görecek
            cmbPaketler.ValueMember = "PaketID";     // Arka planda ID tutulacak


            // 2. ANTRENÖRLERİ MANUEL DOLDURMA
            DataTable dtAntrenorler = new DataTable();
            dtAntrenorler.Columns.Add("AntrenorID", typeof(int));
            dtAntrenorler.Columns.Add("AdSoyad", typeof(string));

            // Elindeki hocaları ID'leriyle buraya ekle:
            dtAntrenorler.Rows.Add(1, "Ahmet Yılmaz (Fitness)");
            dtAntrenorler.Rows.Add(2, "Canan Demir (Pilates)");
            dtAntrenorler.Rows.Add(3, "Mustafa Aydın (Kickbox)");

            cmbAntrenorler.DataSource = dtAntrenorler;
            cmbAntrenorler.DisplayMember = "AdSoyad";    // Kullanıcı adı görecek
            cmbAntrenorler.ValueMember = "AntrenorID"; // Arka planda ID tutulacak
        }

        void listeleme1() //uyeler için method
        {
            if (baglanti.State == ConnectionState.Closed)
            {
                baglanti.Open();
                SqlCommand cmd = new SqlCommand();
                cmd.Connection = baglanti;
                cmd.CommandText = "select*from uyeler";
                SqlDataAdapter adpr = new SqlDataAdapter(cmd);
                DataSet ds = new DataSet();
                adpr.Fill(ds, "uyeler");
                dataGridView1.DataSource = ds.Tables["uyeler"];
                baglanti.Close();


            }
        }

        private void btnUyeEkle_Click(object sender, EventArgs e)
        {
            if (baglanti.State == ConnectionState.Closed)
            {
                baglanti.Open();
                SqlCommand cmd = new SqlCommand();
                cmd.CommandText = "insert into uyeler values ('" + txtUyeAd + "'),('" + txtUyesoyadı + "'),('" + txtuyetelefon + "')";
                cmd.ExecuteNonQuery();
                cmd.Dispose();
                baglanti.Close();
                listeleme1();

            }
        }

        private void btnUyeSil_Click(object sender, EventArgs e)
        {
            if (baglanti.State == ConnectionState.Closed)
            {
                baglanti.Open();
                SqlCommand cmd = new SqlCommand();
                cmd.Connection = baglanti;
                cmd.CommandText = "delete from uyeler where id@numara";
                cmd.Parameters.AddWithValue("@numara", dataGridView1.CurrentRow.Cells[0].Value.ToString());
                cmd.ExecuteNonQuery();
                cmd.Dispose();
                baglanti.Close();
                listeleme1();
            }


        }

        private void btnUyeGuncelle_Click(object sender, EventArgs e)
        {
            if (baglanti.State == ConnectionState.Closed)
            {
                baglanti.Open();
                SqlCommand cmd = new SqlCommand();
                cmd.Connection = baglanti;
                cmd.CommandText = "UPDATE uyeler adı='" + txtUyeAd + "',soyadı='" + txtUyesoyadı + "',telefon='" + txtuyetelefon + "' where id@numara";
                cmd.Parameters.AddWithValue("@numara", dataGridView1.CurrentRow.Cells[0].Value.ToString());
                cmd.ExecuteNonQuery();
                cmd.Dispose();
                baglanti.Close();
                listeleme1();
            }
        }


        void listeleme2() //paketler için method
        {
            if (baglanti.State == ConnectionState.Closed)
            {
                baglanti.Open();
                SqlCommand cmd = new SqlCommand();
                cmd.Connection = baglanti;
                cmd.CommandText = "select*from paketler";
                SqlDataAdapter adpr = new SqlDataAdapter(cmd);
                DataSet ds = new DataSet();
                adpr.Fill(ds, "paketler");
                dataGridView1.DataSource = ds.Tables["paketler"];
                baglanti.Close();


            }
        }

        private void btnPaketEkle_Click(object sender, EventArgs e)
        {
            if (baglanti.State == ConnectionState.Closed)
            {
                baglanti.Open();
                SqlCommand cmd = new SqlCommand();
                cmd.CommandText = "insert into paketler values ('" + txtPaketAdi + "'),('" + txtPaketFiyat + "'),('" + txtPaketSure + "')";
                cmd.ExecuteNonQuery();
                cmd.Dispose();
                baglanti.Close();
                listeleme2();

            }
        }

        private void btnPaketSil_Click(object sender, EventArgs e)
        {
            if (baglanti.State == ConnectionState.Closed)
            {
                baglanti.Open();
                SqlCommand cmd = new SqlCommand();
                cmd.Connection = baglanti;
                cmd.CommandText = "delete from paketler where id@numara";
                cmd.Parameters.AddWithValue("@numara", dgvPaketler.CurrentRow.Cells[0].Value.ToString());
                cmd.ExecuteNonQuery();
                cmd.Dispose();
                baglanti.Close();
                listeleme2();
            }
        }

        private void btnPaketGuncelle_Click(object sender, EventArgs e)
        {
            if (baglanti.State == ConnectionState.Closed)
            {
                baglanti.Open();
                SqlCommand cmd = new SqlCommand();
                cmd.Connection = baglanti;
                cmd.CommandText = "UPDATE paketler paket adı='" + txtPaketAdi + "',Süre(Ay)='" + txtPaketSure + "',Fiyat(TL)='" + txtPaketFiyat + "' where id@numara";
                cmd.Parameters.AddWithValue("@numara", dataGridView1.CurrentRow.Cells[0].Value.ToString());
                cmd.ExecuteNonQuery();
                cmd.Dispose();
                baglanti.Close();
                listeleme2();
            }
        }


        void listeleme3() //antrenor için method
        {
            if (baglanti.State == ConnectionState.Closed)
            {
                baglanti.Open();
                SqlCommand cmd = new SqlCommand();
                cmd.Connection = baglanti;
                cmd.CommandText = "select*from antrenorler";
                SqlDataAdapter adpr = new SqlDataAdapter(cmd);
                DataSet ds = new DataSet();
                adpr.Fill(ds, " antrenorler");
                dataGridView1.DataSource = ds.Tables[" antrenorler"];
                baglanti.Close();


            }
        }

        private void btnAntrenorEkle_Click(object sender, EventArgs e)
        {
            if (baglanti.State == ConnectionState.Closed)
            {
                baglanti.Open();
                SqlCommand cmd = new SqlCommand();
                cmd.CommandText = "insert into antrenorler values ('" + txtAntrenorAdSoyad + "'),('" + txtUzmanlik + "')";
                cmd.ExecuteNonQuery();
                cmd.Dispose();
                baglanti.Close();
                listeleme3();

            }
        }

        private void btnAntrenorSil_Click(object sender, EventArgs e)
        {
            if (baglanti.State == ConnectionState.Closed)
            {
                baglanti.Open();
                SqlCommand cmd = new SqlCommand();
                cmd.Connection = baglanti;
                cmd.CommandText = "delete from antrenorler where id@numara";
                cmd.Parameters.AddWithValue("@numara", dgvAntrenorler.CurrentRow.Cells[0].Value.ToString());
                cmd.ExecuteNonQuery();
                cmd.Dispose();
                baglanti.Close();
                listeleme3();
            }
        }

        private void btnAntrenorGuncelle_Click(object sender, EventArgs e)
        {
            if (baglanti.State == ConnectionState.Closed)
            {
                baglanti.Open();
                SqlCommand cmd = new SqlCommand();
                cmd.Connection = baglanti;
                cmd.CommandText = "UPDATE antrenorler antrenör seçiniz:='" + txtAntrenorAdSoyad + "',uzmanık alanı:='" + txtUzmanlik + "' where id@numara";
                cmd.Parameters.AddWithValue("@numara", dgvAntrenorler.CurrentRow.Cells[0].Value.ToString());
                cmd.ExecuteNonQuery();
                cmd.Dispose();
                baglanti.Close();
                listeleme3();
            }
        }

       
            private void btnSorgula_Click(object sender, EventArgs e)
        {
            try
            {
                // 1. Temel SQL sorgumuzu iki tarih arasına (BETWEEN) göre başlatıyoruz
                // INNER JOIN kullanarak üyenin paket adını ve antrenör adını da ekranda şıkça gösteriyoruz
                string sorgu = @"SELECT u.UyeID, u.Ad, u.Soyad, u.Telefon, p.PaketAdi, a.AdSoyad as Antrenor, u.KayitTarihi 
                         FROM Uyeler u
                         INNER JOIN Paketler p ON u.PaketID = p.PaketID
                         INNER JOIN Antrenorler a ON u.AntrenorID = a.AntrenorID
                         WHERE u.KayitTarihi BETWEEN @Baslangic AND @Bitis";

                // NOT: Eğer sorgu sayfana da paket veya antrenör seçimi için ComboBox koyduysan 
                // sorguyu ValueMember kullanarak şöyle dinamik hale getirebilirsin:
                // sorgu += " AND u.PaketID = @PaketID";

                SqlCommand komut = new SqlCommand(sorgu, baglanti);

                // 2. DateTimePicker bileşenlerinden seçilen tarihleri parametre olarak geçiyoruz
                komut.Parameters.AddWithValue("@Baslangic", dtpBaslangic.Value.Date);
                // Bitiş gününün tüm saatlerini kapsasın diye günün son anına (23:59:59) eşitliyoruz
                komut.Parameters.AddWithValue("@Bitis", dtpBitis.Value.Date.AddDays(1).AddTicks(-1));

                // Eğer yukarıdaki ComboBox filtresini aktif ettiysen ValueMember'ı okuma yöntemi:
                // komut.Parameters.AddWithValue("@PaketID", cmbSorguPaket.SelectedValue); // .SelectedValue direkt ValueMember'ı (ID'yi) verir!

                // 3. Verileri veritabanından çekip DataGridView'e dolduruyoruz
                SqlDataAdapter da = new SqlDataAdapter(komut);
                DataTable dt = new DataTable();
                da.Fill(dt);

                // Sonuçları ekrandaki tabloya basıyoruz
                dgvSorguSonuclari.DataSource = dt;

                // 4. Kullanıcıyı bilgilendirme
                if (dt.Rows.Count == 0)
                {
                    MessageBox.Show("Seçilen kriterlere ve tarih aralığına uygun üye bulunamadı.", "Bilgi", MessageBoxButtons.OK, MessageBoxIcon.Information);
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show("Sorgulama yapılırken bir hata oluştu: " + ex.Message, "Hata", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        

    }
}

}
