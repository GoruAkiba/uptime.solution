UPTIME SOLUTION | Premium
==========================
> Uptime base on Google Spreadsheet and Appsscript service for your BOT project.

Layanan Uptime berbasis pada Google Sreadsheet dan Appsscript untuk projek BOT anda. 

## Geting Started
**Bagaimana cara memulai layanan uptime?**<br>
mudah saja, silahkan registrasikan pengajuan layanan di web kami. Namun pastikan projek ada sudah terintegrasi dengan routing berikut.

**Express Routing for Uptime**
```js
// uptime
app.get("/gb-uptime", (request, response) => {
  const os = require("os"),
        osu = require("os-utils");
  
  osu.cpuUsage(function(v){
    response.json({
      status:"ok",
      sysUptime : p_uptime(os.uptime()),
      uptime: p_uptime(process.uptime()),
      cpuUsage : Math.round(v*1000)/10+"%",
      memUsage: Math.floor((os.totalmem() - os.freemem())/os.totalmem()*1000)/10+"%"
    })
  });
  
});
```
untuk menjalankan routing diatas diperlukan beberapa dependency diantaranya ``express``, dan ``os-utils``.<br>
kemudian fungsi berikut juga diperlukan guna parsing (mengubah) harga dari uptime dalam second menjadi string dengan format yang lebih mudah dimengerti.

```js
function p_uptime(n){
    var t_d = 24*60**2,
        t_h = 60**2,
        d = (n-n%t_d)/t_d,
        h = (n%t_d - n%t_h)/t_h,
        m = (n%t_h - n%60)/60,
        s = Math.floor(n%60);

    return `${d? d+"d ":""}${(d||h)? h+"h ":""}${(h||m)? m+"m ":""}${(m||s)? s+"s":""}`
}
```

  atau keduanya dapat dikombinasikan sebagai berikut:
  ```js
  // uptime
app.get("/gb-uptime", (request, response) => {
    const os = require("os"),
            osu = require("os-utils");
    
    osu.cpuUsage(function(v){
        response.json({
        status:"ok",
        sysUptime : p_uptime(os.uptime()),
        uptime: p_uptime(process.uptime()),
        cpuUsage : Math.round(v*1000)/10+"%",
        memUsage: Math.floor((os.totalmem() - os.freemem())/os.totalmem()*1000)/10+"%"
        })
    });

    // uptime parser
    function p_uptime(n){
        var t_d = 24*60**2,
            t_h = 60**2,
            d = (n-n%t_d)/t_d,
            h = (n%t_d - n%t_h)/t_h,
            m = (n%t_h - n%60)/60,
            s = Math.floor(n%60);

        return `${d? d+"d ":""}${(d||h)? h+"h ":""}${(h||m)? m+"m ":""}${(m||s)? s+"s":""}`
    }
  
});
```
> **Warning:** Tolong untuk tidak mengubah struktur routingnya. karena uptime kami akan bekerja pada path berikut. `<https://your_url_project>/gb-uptime` 

## Server Uptime
**Server Setup**<br>
Untuk pengguna layanan premium. kami akan sediakan 1 server uptime untuk 1 projek. dan kami akan setingkan hingga server dapat berjalan.

**Config Server | Apakah client dapat mengubah config server?**<br>
Tentu saja client dapat mengubah config server. variabel yang dapat diubah adalah ``Project_url.``

**Project_url | Dimana kita dapat mengubah Project_url?**<br>
``Project_Url`` dapat ditemukan pada **Dokumen Report** dan dapat langsung di ubah pada sheet ``Config``.

## Dokumen Report
**Daily Report**<br>
Untuk pengguna layanan premium. kami memiliki fitur **Daily report**.<br>
Dokumen report akan berbentuk google spreadsheet, dan akan setiap saat kami update sejalan dengan berkembangnya waktu.
