function onEdit(e) {
  const editedRange = e.range;
  const sheet = e.source.getActiveSheet();

  const row = editedRange.getRow();
  const col = editedRange.getColumn();

  // Only react to edits in rows 2–9 and columns B–K (which are columns 2–11)
  if (row >= 2 && row <= 9 && col >= 2 && col <= 11) {
    const colLetter = String.fromCharCode(64 + col); // Convert 2 → 'B', 3 → 'C', etc.
    const outputColLetter = String.fromCharCode(64 + col + 10); // e.g., B → L, C → M

    const inputRange = `${colLetter}2:${colLetter}9`;
    const outputRange = `${outputColLetter}2:${outputColLetter}9`;

    assignPointsFromColumn(inputRange, outputRange);
  }
}


function assignPointsToAllColumns() {
  assignPointsFromColumn("B2:B9", "L2:L9"); // B to L
  assignPointsFromColumn("C2:C9", "M2:M9"); // C to M
  assignPointsFromColumn("D2:D9", "N2:N9"); // ...
  assignPointsFromColumn("E2:E9", "O2:O9");
  assignPointsFromColumn("F2:F9", "P2:P9");
  assignPointsFromColumn("G2:G9", "Q2:Q9");
  assignPointsFromColumn("H2:H9", "R2:R9");
  assignPointsFromColumn("I2:I9", "S2:S9");
  assignPointsFromColumn("J2:J9", "T2:T9"); // ...
  assignPointsFromColumn("K2:K9", "U2:U9"); // K to U
}

function assignPointsFromColumn(inputRange, outputRange) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const values = sheet.getRange(inputRange).getValues();

  const indexedValues = values.map((row, i) => ({
    index: i,
    value: Number(row[0]),
  }));

  // Sort ascending (lowest time = best)
  indexedValues.sort((a, b) => a.value - b.value);

  const points = Array(values.length).fill(0);
  let rank = 1;

  for (let i = 0; i < indexedValues.length; ) {
    const currentValue = indexedValues[i].value;
    let tieCount = 1;

    // Count how many are tied at this value
    while (
      i + tieCount < indexedValues.length &&
      indexedValues[i + tieCount].value === currentValue
    ) {
      tieCount++;
    }

    // Calculate the high-biased points for this group
    const point = 9 - rank; // 8 to 1 based on rank
    for (let j = 0; j < tieCount; j++) {
      points[indexedValues[i + j].index] = point;
    }

    rank += 1; // increment rank by 1 regardless of tie count
    i += tieCount;
  }

  const output = points.map(p => [p]);
  sheet.getRange(outputRange).setValues(output);
}

