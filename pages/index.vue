<template>
  <div>
    <b-container>
      <h1>Tạo mã số cho tiệm vàng bảo phương</h1>
      <b-row>
        <b-col cols="6">
          <b-form-group label="Số bắt đầu">
            <b-input
              size="lg"
              type="number"
              min="0"
              v-model="numberStart"
              block
            />
          </b-form-group>
          <b-form-group label="Số kết thúc">
            <b-input
              size="lg"
              type="number"
              min="0"
              v-model="numberEnd"
              block
            />
          </b-form-group>
          <b-button variant="primary" @click="taoma">Tạo mã</b-button>
          <b-button variant="warning" @click="reset">Làm lại</b-button>
        </b-col>
        <b-col cols="6">
          <b-table
            sticky-header="500px"
            :items="tempcsvResult"
            :fields="fields"
            bordered
            small
            show-empty
          >
            <template #cell(maso)="data">
              <b>{{ data.value }}</b>
            </template>
            <template #cell(index)="data">
              <b>{{ data.index + 1 }}</b>
            </template>
          </b-table>
        </b-col>
        <b-col cols="12"> </b-col>
        <b-col cols="12">
          <pre class="text-primary">

          * Cách dùng như sau : 
          1. Nhập vào số bắt đầu : ví dụ 0
          2. Nhập vào số kết thúc : ví dụ 100
          (Lưu ý : số bắt đầu và kết thúc là số giấy bạn để vào khay )
          3. Ấn nút tạo mã, website sẽ tải về 1 file tên là maso.csv
          4. Copy file đó về đặt vào thư mục C:\baophuong ( nếu chưa có thư mục C:\baophuong thì tạo mới thư mục tên là baophuong)
          5. Double click vào file intem trong bảo phương
          6. Ấn Ctrl_P, In Tem
        </pre
          >
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      numberStart: null,
      numberEnd: null,
      fields: [
        { key: "index", label: "Số thứ tự" },
        { key: "maso", label: "Mã số" },
      ],
      csvResult: [],
      tempcsvResult: [],
    };
  },
  methods: {
    createString(num, result = "") {
      if (num === 0) {
        return "0".repeat(6 - result.length) + result;
      }

      const remainder = num % 10;
      const newResult = remainder + result;

      return this.createString(Math.floor(num / 10), newResult);
    },
    download(filename, text) {
      var element = document.createElement("a");
      element.setAttribute(
        "href",
        "data:text/plain;charset=utf-8," + encodeURIComponent(text)
      );
      element.setAttribute("download", filename);

      element.style.display = "none";
      document.body.appendChild(element);
      element.click();
      document.body.removeChild(element);
    },
    taoma() {
      if (this.numberEnd && this.numberStart) {
        //tạo mã
        let range =
          parseFloat(this.numberEnd) - parseFloat(this.numberStart) + 1;
        for (let i = 1; i < range; i++) {
          let n = parseFloat(this.numberStart) + i - 1;
          let result = `*${this.createString(n)}`;
          this.csvResult.push({ maso: result });
        }
        let strHTML = "maso\n";
        for (let i = 0; i < this.csvResult.length; i++) {
          strHTML += `${this.csvResult[i].maso}`;
          if (i == this.csvResult.length - 1) {
          } else {
            strHTML += "\n";
          }
        }
        this.tempcsvResult = this.csvResult;
        this.download("maso.csv", strHTML);
        this.reset();
      } else {
        alert("Vui lòng nhập số Bắt đầu và số kết thúc");
      }
    },
    reset() {
      this.numberEnd = null;
      this.numberStart = null;
      this.csvResult = [];
    },
  },
};
</script>

<style>
</style>
