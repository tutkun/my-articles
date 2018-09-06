 * hasOne / hasMany (1-1, 1-M)
    -save(new or existing child)
    -saveMany(array of models new or existing)
    -create(array of attributes)
    -createMany(array of arrays of attributes)
    ---------------------------------------------------------------------------

 * belongsTo (M-1, 1-1)
    -associate(existing model)
    ---------------------------------------------------------------------------

 *  belongsToMany (M-M)
    -save(new or existing model, array of pivot data, touch parent = true)
    -saveMany(array of new or existing model, array of arrays with pivot ata)
    -create(attributes, array of pivot data, touch parent = true)
    -createMany(array of arrays of attributes, array of arrays with pivot data)
    -attach(existing model / id, array of pivot data, touch parent = true)
    -sync(array of ids OR ids as keys and array of pivot data as values, detach = true)
    -updateExistingPivot(relatedId, array of pivot data, touch)
    ---------------------------------------------------------------------------

 *  morphTo (polymorphic M-1)
    // the same as belongsTo
    ---------------------------------------------------------------------------

 *  morphOne / morphMany (polymorphic 1-M)
    // the same as hasOne / hasMany
    ---------------------------------------------------------------------------

 *  morphedToMany / morphedByMany (polymorphic M-M)
    // the same as belongsToMany
    ---------------------------------------------------------------------------

 *  belongsToManyThrought ?
    // 