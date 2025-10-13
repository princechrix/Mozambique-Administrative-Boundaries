# Mozambique Administrative Boundaries

This JSON file contains the complete administrative structure of Mozambique, organized in a hierarchical format with three administrative levels.

## File Structure

The JSON file follows a nested structure with the following hierarchy:

```
Country → Provinces → Districts → Localities
```

## Administrative Levels

Mozambique has **3 main administrative levels**:

### 1. **Provinces** (Províncias)
- **Total**: 11 provinces
- **Code Format**: `MZ01` to `MZ11`
- **Examples**: Cabo Delgado (MZ01), Gaza (MZ02), Inhambane (MZ03), etc.

### 2. **Districts** (Distritos)
- **Code Format**: `MZ0101`, `MZ0102`, etc. (Province code + 2 digits)
- **Examples**: Ancuabe (MZ0101), Balama (MZ0102), Chiure (MZ0103), etc.

### 3. **Localities** (Localidades)
- **Code Format**: `MZ010101`, `MZ010102`, etc. (District code + 2 digits)
- **Examples**: Ancuabe (MZ010101), Mesa (MZ010102), Metoro (MZ010103), etc.

## JSON Schema

```json
{
  "country": "Mozambique",
  "code": "MZ",
  "provinces": [
    {
      "name": "Province Name",
      "code": "MZ##",
      "districts": [
        {
          "name": "District Name",
          "code": "MZ####",
          "localities": [
            {
              "name": "Locality Name",
              "code": "MZ######"
            }
          ]
        }
      ]
    }
  ]
}
```

## Code Structure

The administrative codes follow a hierarchical pattern:

- **Country**: `MZ`
- **Province**: `MZ` + 2 digits (01-11)
- **District**: Province code + 2 digits (e.g., MZ0101, MZ0102)
- **Locality**: District code + 2 digits (e.g., MZ010101, MZ010102)

## Usage Examples

### Finding a Province
```javascript
const data = JSON.parse(fs.readFileSync('moz_adminboundaries.json', 'utf8'));
const caboDelgado = data.provinces.find(province => province.code === 'MZ01');
```

### Finding Districts in a Province
```javascript
const caboDelgado = data.provinces.find(province => province.code === 'MZ01');
const districts = caboDelgado.districts;
```

### Finding Localities in a District
```javascript
const ancuabeDistrict = caboDelgado.districts.find(district => district.code === 'MZ0101');
const localities = ancuabeDistrict.localities;
```

### Searching by Name
```javascript
function findAdministrativeUnit(name) {
  for (const province of data.provinces) {
    if (province.name.toLowerCase().includes(name.toLowerCase())) {
      return { level: 'province', data: province };
    }
    
    for (const district of province.districts) {
      if (district.name.toLowerCase().includes(name.toLowerCase())) {
        return { level: 'district', data: district, province: province.name };
      }
      
      for (const locality of district.localities) {
        if (locality.name.toLowerCase().includes(name.toLowerCase())) {
          return { level: 'locality', data: locality, district: district.name, province: province.name };
        }
      }
    }
  }
  return null;
}
```

## Data Statistics

- **Total Provinces**: 11
- **Total Districts**: Varies by province
- **Total Localities**: Varies by district
- **File Size**: ~2,670 lines
- **Encoding**: UTF-8

## Notes for Developers

1. **Case Sensitivity**: All names are in title case
2. **Special Characters**: Some names contain accented characters (e.g., "Cidade De Pemba")
3. **Code Uniqueness**: All codes are unique within their respective levels
4. **Hierarchical Integrity**: Each locality belongs to exactly one district, each district to one province
5. **Geographic Coverage**: This covers the entire territory of Mozambique

## File Format

- **Format**: JSON
- **Encoding**: UTF-8
- **Structure**: Nested objects with arrays
- **Validation**: Valid JSON syntax

## Use Cases

This data structure is ideal for:
- Geographic information systems (GIS)
- Address validation systems
- Administrative boundary mapping
- Data analysis and reporting
- Government applications
- Research and academic purposes

## Maintenance

When updating this file:
1. Ensure all codes follow the hierarchical pattern
2. Maintain the nested structure
3. Validate JSON syntax
4. Update this README if the structure changes
