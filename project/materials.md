# Luwte: materials

Prices as shown on the product pages on **15 September 2026**. Reichelt prices are in € incl. 21 % VAT. Pololu prices are in US$ excl. VAT, shipping and import costs.

> **Check Tinytronics yourself before ordering.** The course kit came from there, and one order may be cheaper, but its site blocks automated price checks, so none of its prices are included. Robotshop, Conrad, Mouser, Digikey and TME could not be checked either.

## Already in the course kit

| Item | Used for |
|---|---|
| LilyGO TTGO T3 LoRa32 868 MHz V1.6.1 (ESP32) | Node controller + LoRaWAN radio. Has an OLED (I²C 0x3C), a TP4054 USB charger, battery voltage on GPIO35 and a JST GH 1.25 mm battery cable. **No battery protection**, so use a protected cell. |
| Pololu TB6612FNG dual motor driver (#713) | Motor driver: VMOT 4.5–13.5 V recommended, 1 A continuous per channel, 3 A peak |
| Breadboard + jumper wires | Prototyping |
| MCP23017 I/O expander | Not needed for this project |

## 1. Sensing and actuation (needed for the demo)

| Item | Qty | Product | Supplier | Price | Notes |
|---|---|---|---|---|---|
| Gear motor with encoder | 1 | [100:1 Micro Metal Gearmotor MP 6V, 12 CPR encoder, back connector (#5138)](https://www.pololu.com/product/5138) | Pololu | $29.95 | 220 RPM, 0.67 A stall, well within the TB6612FNG's 1 A |
| Encoder cable | 1 | [JST SH-style cable, 6-pin, 30 cm (#4763)](https://www.pololu.com/product/4763) | Pololu | $3.00 | Not included with the motor |
| Motor bracket | 1 | [Micro Metal Gearmotor Bracket Pair, black (#989)](https://www.pololu.com/product/989) | Pololu | $2.95 | Pair |
| Light sensor | 1 | [DEBO BH 1750 digital light sensor](https://www.reichelt.com/nl/nl/shop/product/developer_boards_-_digitale_lichtsensor_bh1750-224217) | Reichelt | €2,28 | I²C, 3–5 V |
| Outdoor temperature/humidity | 1 | [DEBO BME280](https://www.reichelt.com/nl/nl/shop/product/developer_boards_-_temperatuur-_vochtigheids-_en_druksensor_bm-253982) | Reichelt | €5,95 | **Back in stock 28-9-2026.** Adafruit version in stock now: €19,12 |
| Indoor temperature | 1 | [DEBO LK-TEMP2 DS18B20, waterproof, 1 m](https://www.reichelt.com/nl/nl/shop/product/ontwikkelaarspanelen_-_temperatuursensor_tot_125_c_ds18b20-215884) | Reichelt | €7,11 | No pull-up resistor included |
| 4.7 kΩ resistor | 1 | [Metal film 4,70 kΩ](https://www.reichelt.com/nl/nl/shop/product/metaalfilmweerstand_4_70_k-ohm-11784) | Reichelt | €0,07 | DS18B20 pull-up |
| Limit switches | 2 | [CAMDENBOSS CSM40550F, roller lever](https://www.reichelt.com/nl/nl/shop/product/microschakelaar_250_v_5a_1_wisselcontact_rolhefboom-375538) | Reichelt | €1,94 | €0,97 each; endstops for "fully in" and "fully out" |
| Current sensor | 1 | [DEBO SENS POWER INA219](https://www.reichelt.com/nl/nl/shop/product/ontwikkelaarsboards_-_stroomsensor_met_breakout_board_ina219-266047) | Reichelt | €4,78 | Measures energy per awning movement. Limited stock. |
| Perfboard | 1 | [Rademacher H25PR100, 100 × 100 mm](https://www.reichelt.com/nl/nl/shop/product/breadboard_hardpapier_100x100mm-8270) | Reichelt | €2,19 | For the final build |

### Anemometer

We found **no ready-made cup anemometer** at a shop we could check. The plan is a **3D-printed cup rotor with a hall sensor and a magnet**: one pulse per revolution, calibrated against a reference anemometer.

| Item | Qty | Product | Supplier | Price | Notes |
|---|---|---|---|---|---|
| Hall sensor | 1 | [DEBO SEN HALL (SI7201), digital](https://www.reichelt.com/nl/nl/shop/product/arduino_-_hall-magneetsensor_digitaal-375411) | Reichelt | €3,15 | 2.25–5 V, works at 3.3 V. Clearance stock. |
| Magnets | 1 pack | [Disc magnet 6 × 3 mm, N45, 10 pcs (S-06-03-N)](https://www.supermagnete.nl/S-06-03-N) | Supermagnete | €3,70 | **€20 minimum order.** Any small neodymium magnet works, so check Tinytronics. |
| 3D-printed cups + hub | 1 | Our own design | Fontys 3D printer | — | Plus a small bearing or a smooth axle |

Ready-made alternative: **SparkFun Weather Meter Kit (SEN-15901)**, with a reed-switch anemometer, wind vane and rain gauge. It's listed at Mouser, Farnell and RS, but its price couldn't be verified.

## 2. Power

| Item | Qty | Product | Supplier | Price | Notes |
|---|---|---|---|---|---|
| 18650 cell, protected | 1 | [XTAR 18650-2600, 2600 mAh, button top](https://www.reichelt.com/nl/nl/shop/product/industriele_cel_li-ion_18650_3_6_v_2600_mah_button_top-253361) | Reichelt | €7,53 | Protected cell, needed because the TTGO has no battery protection. 68.5 mm long. |
| 18650 holder with leads | 1 | [MPD BH-18650-W](https://www.reichelt.com/nl/nl/shop/product/batterijhouder_voor_1_18650-213339) | Reichelt | €5,59 | **Check that the 68.5 mm protected cell fits.** Solder to the TTGO's JST GH battery cable. |
| 6 V boost converter with enable | 1 | [Pololu U3V40F6 (#4013)](https://www.pololu.com/product/4013) | Pololu | $9.95 | 1.3–6 V in. When disabled, the battery voltage **passes straight through** to the output, so also hold the TB6612FNG's STBY pin low. |

## 3. Optional

| Item | Qty | Product | Supplier | Price | Notes |
|---|---|---|---|---|---|
| Solar panel | 1 | [Seeed 2.0 W solar panel](https://www.reichelt.com/nl/nl/shop/product/ontwikkelaarsborden_-_zonnepaneel_2_0_w-344081) | Reichelt | €13,63 | 6.4 V at max power, 8.2 V open circuit, 360 mA |
| Solar Li-ion charger | 1 | [Adafruit 390 USB/DC/solar charger](https://www.reichelt.com/nl/nl/shop/product/ontwikkelaar_boards_-_lader_voor_li-ion_lipo_batterijen_usb_d-235495) | Reichelt | €22,32 | **Check the max input before connecting**: the listing says 5–6 V, the panel's open-circuit voltage is 8.2 V. Don't charge from the TTGO's USB at the same time. |
| IP65 enclosure | 1 | [BOX4U 150 × 100 × 60 mm, IP65](https://www.reichelt.com/nl/nl/shop/product/industriele_behuizing_150_x_100_x_60_mm_ip65_lichtgrijs-340533) | Reichelt | €13,83 | Only needed for an outdoor test |
| Rain sensor | 1 | [DEBO SEN RAIN (ME111)](https://www.reichelt.com/nl/nl/shop/product/ontwikkelboards_-_sensormodule_voor_regentrofpen_vocht-282565) | Reichelt | €3,20 | Extra retract trigger; supply voltage not stated |
| JST-XH connectors | 5 / 10 / 5 | [housing](https://www.reichelt.com/nl/nl/shop/product/jst_-_busbehuizing_1x2-polig_-_xh-185085) / contacts / headers | Reichelt | €1,90 | Needs a crimp tool (ask the workshop) |

## 4. Mechanics (buy locally)

These are not priced; get them at a hardware store or the makerspace.

- Wooden dowel or aluminium tube, Ø 12–16 mm: the awning roller
- Piece of (striped) fabric, about 30 × 40 cm
- Plywood/MDF or 3D-printed frame and brackets
- M3 screws, nuts and spacers

## 5. Borrow, don't buy

| Item | Why |
|---|---|
| Multimeter with µA range, or a power profiler | Measure the deep-sleep current |
| Handheld reference anemometer | Calibrate the DIY anemometer |
| Soldering station, crimp tool, 3D printer | Final build |
| Desk fan + lamp | Demo: wind gust and sunshine |

## Totals

| Group | Total |
|---|---|
| Sensing and actuation, Reichelt | €27,47 |
| Anemometer: hall sensor (Reichelt) + magnets (Supermagnete) | €6,85 |
| Power, Reichelt | €13,12 |
| Motor, cable, bracket, boost converter (Pololu) | $45.85 + shipping/import |
| **Required** | **€47,44 + $45.85**, plus shipping (Reichelt from €6,95) |
| Optional | €54,88 |

## Ordering

1. **This week:** check Tinytronics for the same parts; the kit came from there, and one order means one shipping cost.
2. **Order Pololu parts first.** They ship from the US, which takes longest.
3. The BME280 is back in stock on 28-9-2026. Meanwhile, test with the DS18B20.
4. Solar is phase 2 (week 12). Check the charger's input rating before ordering.
