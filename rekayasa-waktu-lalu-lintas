int triggerPin[] = {A0, A2, 2};
int echoPin[] = {A1, A3, 3};

#define LANE_WIDTH 5   // Lebar lajur
#define MAX_DISTANCE 40 // Jarak maksimum (panjang jalur)
 #define TOTAL_TIME 120  // Total waktu dalam detik
#define MIN_ALLOCATION 10  // Waktu alokasi minimum dalam detik
#define MAX_ALLOCATION 60 // Waktu alokasi maksimum dalam detik
#define NUM_LANES 3     // Jumlah lajur
#define MIN_EQUAL_DENSITY 1 // Tingkat kepadatan minimum yang dianggap sama

float distance_1, distance_2, distance_3; // Deklarasi variabel jarak

// Definisikan pin untuk lampu lalu lintas
int merahPin1 = 4;
int kuningPin1 = 5;
int hijauPin1 = 6;

int merahPin2 = 7;
int kuningPin2 = 8;
int hijauPin2 = 9;

int merahPin3 = 10;
int kuningPin3 = 11;
int hijauPin3 = 12;

int buzzer = 13;

unsigned long previousMillis = 0;  // Variabel untuk menyimpan waktu sebelumnya
unsigned long countdownMillis = 0;  // Variabel untuk menyimpan waktu hitungan mundur

void setup() {
  Serial.begin(9600);
  for (int i = 0; i < 3; i++) {
    pinMode(triggerPin[i], OUTPUT);
    pinMode(echoPin[i], INPUT);
  }
  pinMode(merahPin1, OUTPUT);
  pinMode(kuningPin1, OUTPUT);
  pinMode(hijauPin1, OUTPUT);

  pinMode(merahPin2, OUTPUT);
  pinMode(kuningPin2, OUTPUT);
  pinMode(hijauPin2, OUTPUT);

  pinMode(merahPin3, OUTPUT);
  pinMode(kuningPin3, OUTPUT);
  pinMode(hijauPin3, OUTPUT);
  pinMode(buzzer, OUTPUT);
}

float calculateDensity(float distance) {
  float remainingLength = MAX_DISTANCE - distance;
  float densityPercentage = (remainingLength / MAX_DISTANCE) * 100;

  // Batasi nilai persentase kepadatan antara 0% dan 100%
  densityPercentage = max(0, min(100, densityPercentage));

  return densityPercentage;
}

void loop() {
  // Proses scanning selama 5 detik pertama
  for (int i = 0; i < 5; i++) {
    for (int lane = 0; lane < NUM_LANES; lane++) {
      digitalWrite(triggerPin[lane], LOW);
      delayMicroseconds(2);
      digitalWrite(triggerPin[lane], HIGH);
      delayMicroseconds(10);
      digitalWrite(triggerPin[lane], LOW);
      float distance = pulseIn(echoPin[lane], HIGH) * 0.034 / 2;

      // Update nilai variabel jarak sesuai dengan jalur
      switch (lane) {
        case 0:
          distance_1 = distance;
          break;
        case 1:
          distance_2 = distance;
          break;
        case 2:
          distance_3 = distance;
          break;
      }

      // Tampilkan data pada serial monitor
      Serial.print("Jalur ");
      Serial.print(lane + 1);
      Serial.print(": ");
      Serial.print(calculateDensity(distance), 0);
      Serial.print("%, ");
    }
    Serial.println();

    delay(1000);
  }

  // Olah data pada detik keenam
  dataProcessing();

  // Tampilkan pesan pada serial monitor
  Serial.println("Data diolah pada detik keenam");
  delay(1000);
}

void dataProcessing() {
  // Hitung total kepadatan
  float densityTotal = calculateDensity(distance_1) + calculateDensity(distance_2) + calculateDensity(distance_3);

  // Hitung alokasi waktu minimum
  int min_time = MIN_ALLOCATION;
  int equal_time = min(MAX_ALLOCATION, int(TOTAL_TIME / NUM_LANES));

  // Cek apakah semua jalur memiliki kepadatan 0%
  if (abs(densityTotal) < 0.0001) {
    // Berikan alokasi waktu minimal untuk semua jalur
    int time_1 = min(min_time, equal_time);
    int time_2 = min(min_time, equal_time);
    int time_3 = min(min_time, equal_time);

    // Tampilkan waktu alokasi pada serial monitor
    Serial.print("Waktu alokasi untuk Jalur 1: ");
    Serial.print(time_1);
    Serial.println(" detik");

    Serial.print("Waktu alokasi untuk Jalur 2: ");
    Serial.print(time_2);
    Serial.println(" detik");

    Serial.print("Waktu alokasi untuk Jalur 3: ");
    Serial.print(time_3);
    Serial.println(" detik");

    digitalWrite(kuningPin1, HIGH);
    delay(2000);  // Waktu kuning
    digitalWrite(kuningPin1, LOW);
    
    digitalWrite(merahPin1, LOW);
    digitalWrite(merahPin2, HIGH);
    digitalWrite(merahPin3, HIGH);
    digitalWrite(kuningPin2, LOW);
    digitalWrite(kuningPin3, LOW);
    digitalWrite(hijauPin2, LOW);
    digitalWrite(hijauPin3, LOW);

    digitalWrite(hijauPin1, HIGH);
    countdownMillis = millis() + (unsigned long)time_1 * 1000;  // Hitung waktu berakhir hitungan mundur

    // Hitung mundur untuk Jalur 1
    while (millis() < countdownMillis) { //Lampu (R)
      int countdown = (countdownMillis - millis()) / 1000;
      Serial.print("Countdown Jalur 1: ");
      Serial.println(countdown);
      delay(1000);  // Tunggu 1 detik
    }
    digitalWrite(hijauPin1, LOW);

    //-----------------------------------------------

    digitalWrite(kuningPin2, HIGH);
    delay(2000);  // Waktu kuning
    digitalWrite(kuningPin2, LOW);
    
    digitalWrite(merahPin3, HIGH);
    digitalWrite(merahPin2, LOW);
    digitalWrite(merahPin1, HIGH);
    digitalWrite(kuningPin3, LOW);
    digitalWrite(kuningPin1, LOW);
    digitalWrite(hijauPin3, LOW);
    digitalWrite(hijauPin1, LOW);

    digitalWrite(hijauPin2, HIGH);
    countdownMillis = millis() + (unsigned long)time_2 * 1000;  // Hitung waktu berakhir hitungan mundur

    // Hitung mundur untuk Jalur 2
    while (millis() < countdownMillis) { //Lampu (R)
      int countdown = (countdownMillis - millis()) / 1000;
      Serial.print("Countdown Jalur 2: ");
      Serial.println(countdown);
      delay(1000);  // Tunggu 1 detik
    }
    digitalWrite(hijauPin2, LOW);

    //-----------------------------------------------

    digitalWrite(kuningPin3, HIGH);
    delay(2000);  // Waktu kuning
    digitalWrite(kuningPin3, LOW);
    
    digitalWrite(merahPin1, HIGH);
    digitalWrite(merahPin2, HIGH);
    digitalWrite(merahPin3, LOW);
    digitalWrite(kuningPin1, LOW);
    digitalWrite(kuningPin2, LOW);
    digitalWrite(hijauPin1, LOW);
    digitalWrite(hijauPin2, LOW);

    digitalWrite(hijauPin3, HIGH);
    countdownMillis = millis() + (unsigned long)time_3 * 1000;  // Hitung waktu berakhir hitungan mundur

    // Hitung mundur untuk Jalur 3
    while (millis() < countdownMillis) { //Lampu (R)
      int countdown = (countdownMillis - millis()) / 1000;
      Serial.print("Countdown Jalur 3: ");
      Serial.println(countdown);
      delay(1000);  // Tunggu 1 detik
    }
    digitalWrite(hijauPin3, LOW);
    
  } else {
    // Hitung perbandingan kepadatan antar-lajur
    float ratio_1 = calculateDensity(distance_1) / densityTotal;
    float ratio_2 = calculateDensity(distance_2) / densityTotal;
    float ratio_3 = calculateDensity(distance_3) / densityTotal;

    // Hitung alokasi waktu maksimum
    int max_time_1 = min(MAX_ALLOCATION, int(ratio_1 * TOTAL_TIME));
    int max_time_2 = min(MAX_ALLOCATION, int(ratio_2 * TOTAL_TIME));
    int max_time_3 = min(MAX_ALLOCATION, int(ratio_3 * TOTAL_TIME));

    // Hitung alokasi waktu aktual
    int time_1, time_2, time_3;
    // Jika hanya satu jalur yang memiliki kepadatan > 0%, alokasikan waktu berdasarkan rumus t = v/s
    if (densityTotal - max(ratio_1, max(ratio_2, ratio_3)) == 0) {
      time_1 = int(MIN_ALLOCATION + (MAX_ALLOCATION - MIN_ALLOCATION) * ratio_1);
      time_2 = int(MIN_ALLOCATION + (MAX_ALLOCATION - MIN_ALLOCATION) * ratio_2);
      time_3 = int(MIN_ALLOCATION + (MAX_ALLOCATION - MIN_ALLOCATION) * ratio_3);
    // Jika tingkat kepadatan berbeda di bilangan satuannya saja, berikan alokasi waktu minimal
    } else if (abs(int(ratio_1 * 100) - int(ratio_2 * 100)) <= MIN_EQUAL_DENSITY && abs(int(ratio_2 * 100) - int(ratio_3 * 100)) <= MIN_EQUAL_DENSITY) {
      int equal_time = min(MAX_ALLOCATION, int(TOTAL_TIME / NUM_LANES));
      time_1 = time_2 = time_3 = max(min_time, min(equal_time, max_time_1));
    } else {
      // Jika tidak, gunakan alokasi waktu yang dihitung sebelumnya
      time_1 = max(min_time, min(max_time_1, int(ratio_1 * TOTAL_TIME)));
      time_2 = max(min_time, min(max_time_2, int(ratio_2 * TOTAL_TIME)));
      time_3 = max(min_time, min(max_time_3, int(ratio_3 * TOTAL_TIME)));
    }

    // Distribusikan sisa waktu jika total alokasi waktu kurang dari TOTAL_TIME
    int remainingTime = TOTAL_TIME - time_1 - time_2 - time_3;
    if (remainingTime > 0) {
      // Distribusikan sisa waktu berdasarkan perbandingan kepadatan
      float ratio_sum = ratio_1 + ratio_2 + ratio_3;
      time_1 += remainingTime * (ratio_1 / ratio_sum);
      time_2 += remainingTime * (ratio_2 / ratio_sum);
      time_3 += remainingTime * (ratio_3 / ratio_sum);
    }

    // Tampilkan waktu alokasi pada serial monitor
    Serial.print("Waktu alokasi untuk Jalur 1: ");
    Serial.print(time_1);
    Serial.println(" detik");

    Serial.print("Waktu alokasi untuk Jalur 2: ");
    Serial.print(time_2);
    Serial.println(" detik");

    Serial.print("Waktu alokasi untuk Jalur 3: ");
    Serial.print(time_3);
    Serial.println(" detik");

    digitalWrite(kuningPin1, HIGH);
    delay(2000);  // Waktu kuning
    digitalWrite(kuningPin1, LOW);
    
    digitalWrite(merahPin1, LOW);
    digitalWrite(merahPin2, HIGH);
    digitalWrite(merahPin3, HIGH);
    digitalWrite(kuningPin2, LOW);
    digitalWrite(kuningPin3, LOW);
    digitalWrite(hijauPin2, LOW);
    digitalWrite(hijauPin3, LOW);

    digitalWrite(hijauPin1, HIGH);
    countdownMillis = millis() + (unsigned long)time_1 * 1000;  // Hitung waktu berakhir hitungan mundur

    // Hitung mundur untuk Jalur 1
    while (millis() < countdownMillis) { //Lampu (R)
      int countdown = (countdownMillis - millis()) / 1000;
      Serial.print("Countdown Jalur 1: ");
      Serial.println(countdown);
      delay(1000);  // Tunggu 1 detik
    }
    digitalWrite(hijauPin1, LOW);

    //-----------------------------------------------

    digitalWrite(kuningPin2, HIGH);
    delay(2000);  // Waktu kuning
    digitalWrite(kuningPin2, LOW);
    
    digitalWrite(merahPin3, HIGH);
    digitalWrite(merahPin2, LOW);
    digitalWrite(merahPin1, HIGH);
    digitalWrite(kuningPin3, LOW);
    digitalWrite(kuningPin1, LOW);
    digitalWrite(hijauPin3, LOW);
    digitalWrite(hijauPin1, LOW);

    digitalWrite(hijauPin2, HIGH);
    countdownMillis = millis() + (unsigned long)time_2 * 1000;  // Hitung waktu berakhir hitungan mundur

    // Hitung mundur untuk Jalur 2
    while (millis() < countdownMillis) { //Lampu (R)
      int countdown = (countdownMillis - millis()) / 1000;
      Serial.print("Countdown Jalur 2: ");
      Serial.println(countdown);
      delay(1000);  // Tunggu 1 detik
    }
    digitalWrite(hijauPin2, LOW);

    //-----------------------------------------------

    digitalWrite(kuningPin3, HIGH);
    delay(2000);  // Waktu kuning
    digitalWrite(kuningPin3, LOW);
    
    digitalWrite(merahPin1, HIGH);
    digitalWrite(merahPin2, HIGH);
    digitalWrite(merahPin3, LOW);
    digitalWrite(kuningPin1, LOW);
    digitalWrite(kuningPin2, LOW);
    digitalWrite(hijauPin1, LOW);
    digitalWrite(hijauPin2, LOW);

    digitalWrite(hijauPin3, HIGH);
    countdownMillis = millis() + (unsigned long)time_3 * 1000;  // Hitung waktu berakhir hitungan mundur

    // Hitung mundur untuk Jalur 3
    while (millis() < countdownMillis) { //Lampu (R)
      int countdown = (countdownMillis - millis()) / 1000;
      Serial.print("Countdown Jalur 3: ");
      Serial.println(countdown);
      delay(1000);  // Tunggu 1 detik
    }
    digitalWrite(hijauPin3, LOW);
  }
}
