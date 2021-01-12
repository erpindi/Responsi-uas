# Responsi-uas

Nama : Yakobus Epindi Purwanto

NIM  : 19.11.2835

Promos adalah aplikasi pembelian makanan dan minuman yang didalamnya terdapat berbagai jenis promo.
# Fungsi dan kegunaan

`-User dapat melihat daftar makanan dan minuman yang ditawrakan`

`-User dapat memasukan atau menghapus makan dan minuman yang di plih ke dalam keranjang`
 
`-User dapat melihat subtotal makanan atau minuman yang terdapat pada keranjang`
 
 `-User dapat melihat voucher yang ditawarkan`
 
 `-User dapat menggunakan salah satu voucher`
 
 `-User dapat melihat harga total termasuk potongannya`

# Cara Kerjanya
Setelah aplikasi di jalankan dan tampilan awal nya telah di tampilkan. kita bisa memilih atau memesan makanan atau minuman pada menu pesan makanan.

![tampilan pertama](https://user-images.githubusercontent.com/61915300/104317971-5099e500-5511-11eb-988b-cbb08030b567.JPG)

pertama `Penawaran.Xaml.cs` yang bertujuan untuk menawarkan atau menampilkan daftar makanan dan minuman pada listbox.

code :

        private void generateContentPenawaran()
        {
            Item coffeLate = new Item("Coffe Late", 30000);
            Item blackTea = new Item("BlackTea", 20000);
            Item pizza = new Item("Pizza", 75000);
            Item milkShake = new Item("Milk Shake", 15000);
            Item friedRice = new Item("Fried Rice Special", 45000);
            Item watermelonJuice = new Item("Watermelon Juice", 25000);
            Item lemonSquash = new Item("Lemon Squash", 30000);

            Penawarancontroller.addItem(coffeLate);
            Penawarancontroller.addItem(blackTea);
            Penawarancontroller.addItem(pizza);
            Penawarancontroller.addItem(milkShake);
            Penawarancontroller.addItem(friedRice);
            Penawarancontroller.addItem(watermelonJuice);
            Penawarancontroller.addItem(lemonSquash);

            listPenawaran.Items.Refresh();
        }

kemudian  `Voucher.xaml.cs`  yang bertujuan untuk menampilkan berbagai jenis promo pada listbox.

Code:

        private void generateListVoucher()
        {
            Model.Voucher awalTahun = new Model.Voucher(title: "Promo Awal Tahun Diskon 25%", discInPercent: 25);
            Model.Voucher tebusMurah = new Model.Voucher(title: "Promo Tebus Murah Diskon 30% atau max. 30.000", discInPercent: 30);
            Model.Voucher promoNatal = new Model.Voucher(title: "Promo Natal Potongan 10000", disc: 10000);

            voucherController.addItem(awalTahun);
            voucherController.addItem(tebusMurah);
            voucherController.addItem(promoNatal);

            DaftarVoucher.Items.Refresh();
        }


setelah itu `MainWindow.xaml.cs` untuk menginisiasi objek dari `Penawaran.xaml.cs` dan `Voucher.xaml.cs` yang akan di masukan ke keranjang belanja, yang akan ditampilkan pada ListBox.

Code :

        public MainWindow()
        {
            InitializeComponent();

            payment = new Payment(this);
            payment.setBalance(500000);
            payment.setPromo(0);

            KeranjangBelanja keranjangBelanja = new KeranjangBelanja(payment, this);

            controller = new MainWindowController(keranjangBelanja);

            listBoxPesanan.ItemsSource = controller.getSelectedItems();
            listBoxPakaiVoucher.ItemsSource = controller.getSelectedVouchers();

            initializeView();

        }

        private void initializeView()
        {
            labelSubtotal.Content = 0;
            labelGrantTotal.Content = 0;
            labelPromoFee.Content = (payment.getPromo() > 0) ? - payment.getPromo() : 0;
        }

        public void onPenawaranSelected(Item item)
        {
            controller.addItem(item);
        }

        private void onButtonAddItemClicked(object sender, RoutedEventArgs e)
        {
            Penawaran penawaranWindow = new Penawaran();
            penawaranWindow.SetOnItemSelectedListener(this);
            penawaranWindow.Show();
        }

untuk menghapus makanan atau minuman yang telah dimasukan ke keranjang belanja.

code:
        private void listBoxPesanan_ItemClicked(object sender, MouseButtonEventArgs e)
        {
            if (MessageBox.Show("Kamu ingin menghapus item ini?",
                    "Konfirmasi", MessageBoxButton.YesNo) == MessageBoxResult.Yes)
            {
                ListBox listBox = sender as ListBox;
                Item item = listBox.SelectedItem as Item;
                controller.deleteSelectedItem(item);
            }
        }
        
Tampilan:
![menghapus makanan](https://user-images.githubusercontent.com/61915300/104331936-3e747280-5522-11eb-8e70-b3ec9bdbfdf8.JPG)


# Simulasi 1
pada simulasi pertamana makanan dan minuman yang ditampilkan adalah Coffe Late, Pizza dan Lemon Squash dengan memakai Diskon `Promo Awal Tahun Diskon 25%`

![simpulasi 1](https://user-images.githubusercontent.com/61915300/104318228-b1292200-5511-11eb-83ec-b28fb5ab914a.JPG)

# Simulasi 2
pada simulasi pertamana makanan dan minuman yang ditampilkan adalah Coffe Late, Pizza dan Lemon Squash dengan memakai Diskon `Promo Tebus Murah Diskon 30% atau maksimal 30.000`

![simulasi 2](https://user-images.githubusercontent.com/61915300/104318710-5a701800-5512-11eb-866a-f3fc35b018aa.JPG)


# Simulasi 3
pada simulasi pertamana makanan dan minuman yang ditampilkan adalah Coffe Late, Pizza dan Lemon Squash dengan memakai Diskon `Promo Natal Potongan 10.000`

![simulasi 3](https://user-images.githubusercontent.com/61915300/104318762-72479c00-5512-11eb-9d87-7bc8f588681f.JPG)
