# VSM Laravel (Cosine Measurement)
Module VSM untuk membantu pencarian pada laravel menggunakan Information retrieval berbahasa indonesia.

## Setting Up
Untuk menggunakan model ini anda harus melakukan beberapa langkah setup.
* Import table [tb_katadasar.sql](http://www.google.com) ke database anda.
* Buat folder **Helper** dibawah folder *app/*
* Letakan **VSMProvider.php** dan **PreprocessingProvider.php** dibawah folder *app/Provider*

### Penggunaan
Setelah melakukan setup anda dapat menggunakan pada controller.
Berikut contoh penggunaannya:
```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\DB;
// helper VSM
use App\Helpers\PreprocessingHelper as Preprocessing;
use App\Helpers\VSMHelper as VSM;

class KriminalController extends Controller
{
	// ====== PENGGUNAAN ====== //
    public function pencarian(Request $request)
    {
        // STEP 1 == query ke kata dasar (array)
        $query_dasar = Preprocessing::preprocess($request->cari);

        // STEP 2 = =get dokumen & parse to array
        $dokumen = DB::table('artikel')->get();
        $arrayDokumen = [];
        foreach ($dokumen as $d) {
            $arrayDoc = [
                "id_doc" => $d->id_blog,
                "dokumen" => implode(" ", Preprocessing::preprocess($d->deskripsi)),
            ];
            array_push($arrayDokumen, $arrayDoc);
        }

        // STEP 3 == get rank
        $rank = VSM::get_rank($query_dasar, $arrayDokumen);

        dd($rank);
    }
}
```

## Authors

* **Andri Laksono** - *Programmer* - [Satria Tech](https://satriatech.com)

## License

This project is licensed under the MIT License
