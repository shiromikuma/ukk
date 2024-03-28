# ukk
#include <stdio.h>

#include <stdlib.h>

#include <time.h>



#define MAX_PRODUCTS 5



struct Product{

    char nama[50];

    int harga;

    int jumlah;

};



void tampilkanProducts(struct Product products[]){

    int i;

    printf("=====================================\n");

    printf("| No. | Nama Barang     | Harga     |\n");

    printf("=====================================\n");

    for(i = 0; i < MAX_PRODUCTS; i++){

        printf("| %-3d | %-15s | Rp.%-6d |\n", i + 1, products[i].nama, products[i].harga);

    }

    printf("=====================================\n");

}



int hitungTotal(struct Product products[]){

    int total = 0;

    int i;

    for(i = 0; i < MAX_PRODUCTS; i++){

        total += products[i].harga * products[i].jumlah;

    }

    return total;

}



int hitungDiskon(struct Product products[]){

    int totalDiskon = 0;

    int i;

    for(i = 0; i < MAX_PRODUCTS; i++){

        if(products[i].jumlah > 3 && products[i].jumlah <= 5){

            totalDiskon += (products[i].harga * products[i].jumlah) * 0.10;

        }else if(products[i].jumlah > 5){

            totalDiskon += (products[i].harga * products[i].jumlah) * 0.15;

        }

    }

    return totalDiskon;

}



void resetPilihan(struct Product products[]){

    int i;

    for(i = 0; i < MAX_PRODUCTS; i++){

        products[i].jumlah = 0;

    }

}



void cetakStrukPembayaran(struct Product products[], int uang){

    FILE *receiptFile = fopen("C:/Users/HP/Downloads/struk_laba.txt", "w");

    int i, totalHarga, totalDiskon, totalBayar, uangPembayaran, kembalian;

    time_t now = time(NULL);

    struct tm tm = *localtime(&now);



    fprintf(receiptFile, "|========================================================================|\n");

    fprintf(receiptFile, "|                              Toko SKENSA                               |\n");

    fprintf(receiptFile, "|                 Jalan HOS Cokroaminoto No. 84 Denpasar                 |\n");

    fprintf(receiptFile, "|                                  Bali                                  |\n");

    fprintf(receiptFile, "|                           Telp : 082266551815                          |\n");

    fprintf(receiptFile, "| ID Struk : 12345667                                                    |\n");

    fprintf(receiptFile, "|========================================================================|\n");



    int jumlahBarang = 0;

    for (i = 0; i < MAX_PRODUCTS; i++) {

        if (products[i].jumlah > 0) {

            jumlahBarang++;

            break;

        }

    }



    if(jumlahBarang > 0){

        fprintf(receiptFile,"|========================================================================|\n");

        fprintf(receiptFile,"| No. | Nama Barang     | Harga     | Jumlah    | Diskon    | Total Harga|\n");

        fprintf(receiptFile,"|========================================================================|\n");



        for(i = 0; i < MAX_PRODUCTS; i++){

            if(products[i].jumlah > 0){



                //Nyari subtotal & diskon khusus untuk rekap struk

                int subtotal;

                int diskon = 0;

                subtotal = products[i].harga * products[i].jumlah;

                if(products[i].jumlah > 3 && products[i].jumlah <= 5){

                    diskon += (products[i].harga * products[i].jumlah) * 0.10;

                }else if(products[i].jumlah > 5){

                    diskon += (products[i].harga * products[i].jumlah) * 0.15;

                }

                fprintf(receiptFile,"| %-3d | %-15s | Rp.%-6d | %-9d | Rp.%-6d | Rp.%-7d |\n", i + 1, products[i].nama,                products[i].harga, products[i].jumlah, diskon, (subtotal - diskon));

            }

        }



        totalHarga = hitungTotal(products);

        totalDiskon = hitungDiskon(products);

        totalBayar = totalHarga - totalDiskon;

        uangPembayaran = uang;

        kembalian = uangPembayaran - totalBayar;



        fprintf(receiptFile,"|========================================================================|\n");

        fprintf(receiptFile,"|                                                                        |\n");

        fprintf(receiptFile,"| Total Harga : %d                                                    |\n", totalHarga);

        fprintf(receiptFile,"| Total Diskon : %d                                                    |\n", totalDiskon);

        fprintf(receiptFile,"| Total Pembayaran : %d                                               |\n", totalBayar);

        fprintf(receiptFile,"| Uang Pembayaran : %d                                                |\n",              uangPembayaran);

        fprintf(receiptFile,"| Kembalian : %d                                                       |\n", kembalian);

        fprintf(receiptFile,"| Tanggal dan Waktu: %d-%02d-%02d %02d:%02d:%02d                                 |\n", tm.tm_year +        1900, tm.tm_mon + 1, tm.tm_mday,tm.tm_hour, tm.tm_min, tm.tm_sec);

        fprintf(receiptFile,"|========================================================================|\n");



    }





    fclose(receiptFile);

}





void strukPembayaran(struct Product products[]){

    int totalHarga, totalDiskon, totalPembayaran, i, uangBayar, Kembalian;

    char confirm;



    totalHarga = hitungTotal(products);

    totalDiskon = hitungDiskon(products);

    totalPembayaran = totalHarga - totalDiskon;



    int jumlahBarang = 0;

    for (i = 0; i < MAX_PRODUCTS; i++) {

        if (products[i].jumlah > 0) {

            jumlahBarang++;

            break;

        }

    }



    if(jumlahBarang > 0){

        printf("=============================================================================\n");

        printf("| No. | Nama Barang     | Harga     | Jumlah    | Diskon    | Total Harga   |\n");

        printf("=============================================================================\n");

        for(i = 0; i < MAX_PRODUCTS; i++){

            if(products[i].jumlah > 0){



                //Nyari subtotal & diskon khusus untuk rekap struk

                int subtotal;

                int diskon = 0;

                subtotal = products[i].harga * products[i].jumlah;

                if(products[i].jumlah > 3 && products[i].jumlah <= 5){

                    diskon += (products[i].harga * products[i].jumlah) * 0.10;

                }else if(products[i].jumlah > 5){

                    diskon += (products[i].harga * products[i].jumlah) * 0.15;

                }

                printf("| %-3d | %-15s | Rp.%-6d | %-9d | Rp.%-6d | Rp.%-10d |\n", i + 1, products[i].nama,                products[i].harga, products[i].jumlah, diskon, (subtotal - diskon));

            }

        }

        printf("=============================================================================\n");



        printf("Yakin dengan pesanan anda?(y/n) : ");

        scanf("%s", &confirm);



        if(confirm != 'Y' && confirm != 'y'){

            printf("\n\n");

            return;

        }



        printf("total harga : %d\n", totalHarga);

        printf("total diskon : %d\n", totalDiskon);

        printf("total pembayaran : %d\n", totalPembayaran);



        printf("=============================================================================\n\n");

        printf("Masukkan uang bayar : ");

        scanf("%d", &uangBayar);



        if(uangBayar < totalPembayaran){

            printf("Uang anda kurang, silahkan ambil uang dulu!!\n");

            return;

        }



        Kembalian = uangBayar - totalPembayaran;

        printf("Kembalian : Rp.%d\n", Kembalian);

        cetakStrukPembayaran(products, uangBayar);

        resetPilihan(products);

        printf("TERIMA KASIH SUDAH BERBELANJA!!\n\n");



    }else{

        printf("Anda belum memesan apapun!!\n\n");

    }

}



int main()

{

    struct Product products[MAX_PRODUCTS] = {

        {"Buku Tulis", 5000, 0},

        {"Pensil", 2000, 0},

        {"Penghapus", 1000, 0},

        {"Penggaris", 1000, 0},

        {"Bujur Sangkar", 500, 0}

    };

    int pilihan;



    do{

        printf("SELAMAT DATANG DI TOKO SKENSA\n");

        printf("Silahkan pilih barang yang anda inginkan :\n\n");

        tampilkanProducts(products);



        printf("\n");

        printf("99. Struk Pembayaran\n");

        printf("55. Reset Pilihan\n");

        printf("00. Keluar\n\n");

        printf("=====================================\n\n");

        printf("input pilihan yang anda inginkan : ");

        scanf("%d", &pilihan);



        if(pilihan >= 1 && pilihan <= MAX_PRODUCTS){

            printf("Masukkan jumlah barang yang anda inginkan : ");

            scanf("%d", &products[pilihan-1].jumlah);

            printf("%s berhasil ditambahkan ke keranjang!!\n\n", products[pilihan-1].nama);

        }else if(pilihan == 99){

            strukPembayaran(products);

        }else if(pilihan == 55){

resetPilihan(products);      

  }else if(pilihan != 00){

            printf("Input tidak valid, silahkan coba lagi!\n");

        }

    } while(pilihan != 00);



    return 0;

}

